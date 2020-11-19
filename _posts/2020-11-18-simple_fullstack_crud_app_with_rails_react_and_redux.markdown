---
layout: post
title:      "Simple Fullstack CRUD App With Rails, React &Redux"
date:       2020-11-19 03:35:05 +0000
permalink:  simple_fullstack_crud_app_with_rails_react_and_redux
---


<div>Working with a server-side language and a front-end framework to build persistent web applications can be very tricky. This guide will cover how to build a JSON API with Ruby on Rails and build a functional React and Redux front-end that interact with the API. We will be building a single page application, question and answer engine for web developer learners to post questions about the challenge they are facing while using HTML, CSS, JavaScript and Rails, and also provide insights and clarity by answering other users’ questions.</div><br/><br/>

<div>The app demonstrates basic user authentication with <span style="color:red">**JWT authentication**</span> and **CRUD **(Create, Read, Update, Delete) functionality, as well as couple extra features like filter, flash message and reaction buttons. We will be working with three models: User, Question, and Answer. This guide is divided into three parts.  Part 1 will cover implementing user registration, login and logout functionality in the server (Rails API) with JWT, part 2 will cover auth using JWT in the client side (React + Redux), and part 3 will cover the creating, reading , updating and deleting a question.</div>

## Part 1: User Authentication

We will be using a basic approach to implement  <span style="color:red"> JSON Web Tokens</span> <strong>(JWT)</strong>, setting up both our rails API and React to handle the generated tokens.

### 	Rails API

The complete code for my rails API can be found <a href= 'https://github.com/vanessuniq/rails-qa-engine-api' style="color:indigo; font-size:16px">here</a>.

Run the following on your terminal to create your rails api:
```
rails new your_app_project_name --api --database=postgresql
```

We will need to add 2 essentials gems for our authentication `gem 'bcrypt'` and `gem 'jwt'` to our gem file. We will also add `gem 'active_model_serializers'` to render JSON with associated objects. You can learn more about active model serialization <a href='https://learn.co/lessons/using-active-model-serializer'>following the steps in this guide</a>.
Finally do not forget to uncomment `rack-cors` in the gemfile. This will  make our AJAX requests possible. you can read more about it <a href="https://github.com/learn-co-curriculum/mod3-project-week-setup-example">here</a>.

Now you can go ahead and call `bundle install` on your terminal. 

My complete <span style='color:blue'>Gemfile</span> looks like this:

```ruby

source 'https://rubygems.org'
git_source(:github) { |repo| "https://github.com/#{repo}.git" }

ruby '2.6.1'
gem 'dotenv-rails', groups: [:development, :test]
# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem 'rails', '~> 6.0.3', '>= 6.0.3.2'
# Use postgresql as the database for Active Record
gem 'pg', '>= 0.18', '< 2.0'
# Use Puma as the app server
gem 'puma', '~> 4.1'
# Build JSON APIs with ease. Read more: https://github.com/rails/jbuilder
# gem 'jbuilder', '~> 2.7'
# Use Redis adapter to run Action Cable in production
# gem 'redis', '~> 4.0'
# Use Active Model has_secure_password
gem 'bcrypt', '~> 3.1.7'
gem 'active_model_serializers'
gem 'jwt'
gem 'faker'

# Use Active Storage variant
# gem 'image_processing', '~> 1.2'

# Reduces boot times through caching; required in config/boot.rb
gem 'bootsnap', '>= 1.4.2', require: false

# Use Rack CORS for handling Cross-Origin Resource Sharing (CORS), making cross-origin AJAX possible
gem 'rack-cors'

group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
end

group :development do
  gem 'listen', '~> 3.2'
  # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
  gem 'spring'
  gem 'spring-watcher-listen', '~> 2.0.0'
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]

```

Next enable CORS in your app by uncommenting the following in config/initializers/cors.rb and changing the origins from example.com to * :  mine looks like this

```ruby

 Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end

```

Now we need to create our User model, controller, routes, and create our data base. For that, run the following:

* `rails g model User username password_digest bio avatar`  this will also create the user table in the db
* `rails g controller api/v1/users`
* `rails g serializer user`
* `rails db:create`
*` rails db:migrate`

#### User Model

```ruby

class User < ApplicationRecord
    before_save :downcase_username
    has_secure_password
    
    validates :username, uniqueness: {case_sensitive: false}, length: {minimum:4}
    validates :password, presence: true, length: { minimum: 5}, allow_nil: true

    private

    def downcase_username
         self.username = self.username.downcase
    end

end

```

`has_secure_password` comes from ActiveModel and adds methods to set and authenticate against a BCrypt password. I also added some validation for user to have unique username with a minimum length of 4 and required the password to be at least 5 characters. As we can see in the private methode I am also converting to username to lower case before saving it in the database.

#### User Serializer

Again, this is an easy way to structure the data we want rails to return to our fornt side.

```ruby

class UserSerializer < ActiveModel::Serializer
  attributes :id, :username, :bio, :avatar
end

```

Notice we are omitting to return the password for security reason.

#### Routes

The app has the following routes for users:

```ruby

Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
		
      resources :users, only: [:create, :index]   #handle signup, query users
      post '/login', to: "auth#login"                       #handles login for existing users
      get '/auto_login', to: 'auth#auto_login'    #handles auto login when user revisit the app
      
    end
  end
  
end

```

#### User Registration / Sign Up

Now it's time to implement authentication with JWT.  Here is the flow for  registration: 

1.  a new user fill out the sign up form on the client side with valid username and password and submit the form
2.  a POST request is made to <span style="color:green">/api/v1/users</span> which is redirected to the <span style="color:green">create</span> action of the User Controller
3.  if an instance of user is successfully created, the server issues an encoded token to the client
4.  the client then save the token into the <span style="color:green">localestorage</span> and presents it with every request to authenticate the user on the server side

```ruby

class Api::V1::UsersController < ApplicationController
    skip_before_action :require_login
		
    def create
        user = User.create(user_params)
        if user.valid?
            payload = {user_id: user.id}
            token = encode_token(payload)
            render json: {user: UserSerializer.new(user), jwt: token}, status: :created
        else
            render json: {errors: user.errors.full_messages}, status: :not_acceptable
        end
    end

    private

    def user_params
        params.require(:user).permit(:username, :password, :bio, :avatar)
    end
end


```

Once the an instance of user is successfully created, a payload object or hash is created with the user'id and is encoded using the `encode_token` method.
Since different controllers will need to authenticate and authorize users, it makes sense that our top level ApplicationController handles the functionality of encoding/decoding tokens so that it can pass it down to all controllers.

This is how the method looks like in the ApplicationController

```ruby

def encode_token(payload)
        JWT.encode(payload, 'my_secret')
 end
 
```

the JWT gem provides the `encode` method which we use to generate the token. This method takes up to three arguments: a payload to encode, an application secret (of your choice), and an optional third that can be used to specify the hashing algorithm used. The return value is a JWT as a string having the following structure

`"eyJhbGciOiJIUzI1NiJ9.eyJiZWVmIjoic3RlYWsifQ._IBTHTLGX35ZJWTCcY30tLmwU9arwdpNVxtVU0NpAuI"`

upon successful registration, the server renders the user object and the JWT (key/value pairs)  as a JSON object. If registration failed, the server renders the list of errors as a JSON object.

This is a fetch example for signing up:

```js

fetch('http://localhost:3000/api/v1/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    Accept: 'application/json'
  },
  body: JSON.stringify({
    user: {
      username: "railsapi",
      password: "whatscooking",
      bio: "I am a lovable developer",
      avatar: "https://media.istockphoto.com/vectors/user-sign-icon-person-symbol-human-avatar-vector-id639085714"
    }
  })
})
  .then(r => r.json())
  .then(console.log).catch(error => console.log(error.errors[0]))
```

#### User Login

logging in follows a similar approach as signing up, except that the user does not need to be recreated, but must authenticate with valid credentials.  This is handled by the `login` action of the **Auth Controller**.

in **app/controllers/api/v1/auth_controller.rb

```ruby

def login
        user = User.find_by(username: login_params[:username])
        if user && user.authenticate(login_params[:password])
            payload = {user_id: user.id}
            token = encode_token(payload)
            render json: {user: UserSerializer.new(user), jwt: token}, status: :accepted
        else
            render json: {failure: "Invalid username or password"}, status: :unauthorized
        end
end

private

    def login_params
        params.require(:user).permit(:username, :password)
    end
		
```

Here, the user send its credentials (username and password) to the server, then the server lookup the user by username using the active record `find_by` method. If there's a username match in the database, the server then compares the provided password to the one stored in the database using the `authenticate` method provided by `bcrypt`.. If the user is successfully authenticated, JWT generates a token which is rendered to the client side as well as the user instance in a JSON format.

#### Auto Login (User Revisiting the App)

When the users revisit the app, we will them to continue their session from before if they have a token saved in their locale storage (**the whole point of saving the generated token**). Because when using tokens the server does not save sessions, thus does not remember the current user, the client will need to provide the token saved in the locale storage for the server to lookup and render the current user.

<a href='https://jwt.io/introduction/'>Referring to the JWT documentation</a>: Whenever the user wants to access a protected route or resource, the user agent (browser in our case) should send the JWT, typically in the Authorization header using the Bearer schema. The content of the header should look like the following:
`Authorization: Bearer <token>`

Let examine the code for auto login:

In **app/controllers/api/v1/auth_controller.rb**

```ruby

class Api::V1::AuthController < ApplicationController
    skip_before_action :require_login, only: [:login]

    def login
        user = User.find_by(username: login_params[:username])
        if user && user.authenticate(login_params[:password])
            payload = {user_id: user.id}
            token = encode_token(payload)
            render json: {user: UserSerializer.new(user), jwt: token}, status: :accepted
        else
            render json: {failure: "Invalid username or password"}, status: :unauthorized
        end
    end

    def auto_login
        render json: {user: UserSerializer.new(current_user)}, status: :accepted
    end
    
    private

    def login_params
        params.require(:user).permit(:username, :password)
    end
    
end

```

auto login seems pretty straight forward. It renders the current user. However, the `auto_login` action cannot be accessed unless the method `require_login` allows it. This pattern is enforced by the macro `before_action` that instructs the API to run the `require_login` method before any action, unless skipped (`skip_before_action :require_login, only: [:login]`) like it is the case for the `login` action. It will not make sense to require the user to be logged in before logging in. 

Let explore our `current_user` and `require_login` methods in the Application Controller, as well as all the jwt functionalities.  Below is the final Application Controller

```ruby

class ApplicationController < ActionController::API
    before_action :require_login
		
    def encode_token(payload)
        JWT.encode(payload, 'my_secret')
    end

    def auth_header
        request.headers['Authorization']
    end
    
    def decoded_token
        if auth_header
            token = auth_header.split(' ')[1]
            begin
                JWT.decode(token, 'my_secret', true, algorithm: 'HS256')
            rescue JWT::DecodeError
                nil
            end
        end
        
    end

    def current_user
        
        if decoded_token
				
            user_id = decoded_token[0]['user_id']
            @user = User.find_by(id: user_id)
       
        end
    end
    
    def logged_in?
        !!current_user  # ruby object/class instance is 'truthy'
    end
    
    def require_login
        render json: {message: 'Please Login or Sign up to see content'}, status: :unauthorized unless logged_in?
    end
    
end
		
```

The `require_login` method returns an error message <span style="color:red">'Please Login or Sign up to see content'</span> unless the user is logged in. The `logged_in` method check if there is a current user, which is obtained by decoding the token provided by the client when sending the request . The server uses the `decode` method provided by JWT gem to decode the token and obtain the user_id that is used to look up the current user.

The `JWT.decode` takes three arguments: a JWT as a string, an application secret, and––optionally––a hashing algorithm.
We set up our server (by defining the `auth_header`) to anticipate a JWT sent along in request headers, instead of passing the token directly to `ApplicationController#decoded_token`

Normally, if the server receives and attempts to decode an invalid token, the `decode` method will raise an exception causing a 500 Internal Server Error. The `Begin/Rescue` syntax prevents us from crashing the server, allowing us to rescue out of an exception in Ruby. Thus, if the server recieves an invalid token, the `decoded_token` will simply returns `nil`.

The header obtained from the fetch is converted into an array with the first element being the `Bearer` and the second element the jwt-token. Only the jwt-token is needed here to decode into the user_id. 

Again note the `before_action :require_login` set in the application controller which automatically lock down the application unless the client provides a valid token or the before action is set to be skipped for a specific action. 

This is it for the basic implementation of JWT in  a Rails API.  Please read  more about JWT and other approaches  <a href='https://jwt.io/introduction/'>here</a>.

We will be covering JWT implementation in our client-side (**React & Redux**) in my next post.

Until then, Happy Coding!!

