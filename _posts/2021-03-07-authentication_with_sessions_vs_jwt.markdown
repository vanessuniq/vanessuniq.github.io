---
layout: post
title:      "Authentication with Sessions Vs. JWT"
date:       2021-03-08 02:55:16 +0000
permalink:  authentication_with_sessions_vs_jwt
---


I was recently asked what the difference was between using JWT and sessions for authentication in an interview; and I thought I will write about it to cement that knowledge.

**HTTP** is known to be a stateless protocol used to enable a communication or transfer of data between two or more machines, mainly the communication between the client and server side. 
Due to the stateless property of HTTP, it is unable to remember state of our application, like remembering if a user is currently logged in for example.  Sessions and tokens are used to overcome this challenge.

## Session Based Authentication

A session gives web applications the ability to set a cookie that persists a user across multiple HTTP requests. Here is the flow of creating and persisting a session:

You browse a website like twitter.com for example and submit your login credentials. Twitter's server checks to see if your credentials are correct. If they are, Twitter's server issues a cookie to your browser and save your session in the database. If you visit another on twitter.com like your profile page as an example, your browser will show the cookie to the server. The server verifies the presented cookie, if recognized, the server lets you load and see your recent activities. Your session will persist until you explicitly logout or your session expires (given that the expiration time was set).

Let's implement it with Rails:


We will define a simple login system that only requires the email for this demo. The system's flow is as follow: 
*  The route: `get '/login'` that will present a form to the user to enter his/her email
*  The user enters the email and submit the form
*  The form is posted to `post '/login'`
*  The request is handled in the `create` action of the `SessionsController`.  If the submitted email is found in the database, we set a cookie on the user's browser by writing their `id` into the session object.
*  This will thereafter persist the user with subsequent request to the server

Let's create the routes in `routes.rb`

```ruby

Rails.application.routes.draw do
  get “/login”, to: “sessions#new”
  post “/login”, to: “sessions#create”
end
```

Here is the code for the Sessions controller in the `controller.rb`. We will assume that we have a `User` model predefined

```ruby

class SessionsController < ApplicationController
    def new
        # this will simply render the new.html.erb file containing the login form
    end

    def create
		 user = User.find_by(email: params[:email]
		 if user
			 session[:id] = user.id
		end
        redirect_to '/'
    end
		
end

```

In the above code, the `create` action lookup the user's email in the database. If the email is found, it then stores the authenticated user's id in the session then redirect to the main page.

There's a sample code for the form in the `new.html.erb` file

```html
<form method='post'>
  <input name='email'>
  <input type='submit' value='login'>
</form>
```

Now when the user submits the form and the server authenticates the user, the user will stay logged in.

## Token Based Authentication

`JSON Web Tokens` (JWTs) are widely used for **RESTful APIs**. Here, the token is stored on the client side, usually in the `localStorage`. That token will serve as a pass to the server for every user's requests for secured data. The picture below describes the flow with JWT:
![](https://hackernoon.com/images/pazJZnCJTqSZxQS4tltZo4Gatbo1-fo8h3yl1.jpg)

We can see here that when the user posts his login credentials, the server creates an encrypted token in the form of JSON Web Token (JWT) and sends it back to the client. When the client receives a token, it means that the user is authenticated to perform any activity using the client. You can check how JWT is implemented in Rails in [this previous post](https://vanessuniq.medium.com/simple-fullstack-crud-app-with-rails-react-redux-54a9d49d8ac3).

## Final Note
Tokens are saved on the client side while Sessions are stored on the browser as cookie and use the server memory to store user data. Storing data on the server's memory can slow down an application if multiple users access it at the same time. From this point of view, token-based authentication appeared to be more scalable.

I hope this post is helpful. Please reach out with any feedback.

Until next time,

Happy Coding!!
