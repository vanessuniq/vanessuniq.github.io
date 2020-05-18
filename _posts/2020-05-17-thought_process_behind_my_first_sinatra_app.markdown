---
layout: post
title:      "Thought Process Behind My First Sinatra App"
date:       2020-05-18 03:15:24 +0000
permalink:  thought_process_behind_my_first_sinatra_app
---


The idea of building my first web application was intimidating. The task was to build  a simple Content Management 

System (CMS) using the sinatra tools I have been learning and practicing in Flatiron's Software Engineering program. 

Sinatra is a simple web framework written in Ruby that provides us with the request and allows us to generate the 

response we want. The resulting app should have many models, allow users to create their own pages, view the pages

they’ve created, edit and delete their existing pages. 

![](https://keep.google.com/u/0/media/v2/1Y8MYqwH8qGuDMJowrKYxBZOi7E9a2X94YvpLdROKf8d4CtHqyAOBUhlcW2Vm7Q/15EfD2MTj8cLNXPPhh5TXT0bS6fQ0OLtAxd4GRoTe0RR_LbXZxe5mWICePp7dx4c?accept=image/gif,image/jpeg,image/jpg,image/png,image/webp,audio/aac&sz=1003)


This is how I planned my project:

## **Generate Ideas On What To Build and Set Expectation (pseudocoding)**

After brainstorming all possible things I could track, I settled with creating an application that can help users to set and

track their workout challenges. I named it **GetFit**. I identified four models for the app: User, Challenge, Type, and Day.

**My expectation for the app:**

1. The app should allow users to first sign up if they do not have an account yet, then sign in and sign out
2. The app should validate the uniqueness of the login attributes. If failure, the app should generate an error message and prompt the user to validate the login info or sign up
3. The user should be able to create/ set one or many workout challenges and give it a name, duration, start_date and end_date. 
4. The user should be able to view, edit or delete his/her challenge(s)
5. Each challenge page should show exercise details for each day

## **Build an Entity Relationship Diagram for My Models **

![](https://keep.google.com/u/0/media/v2/14muL-eQUUicKW1SoLfcLZ8M2-vKLNLJc1sYPQgyhoMxTIIeKDZONWXxtF3TwOes/1EMwWOIVgHe2ztE8Hw2W49XzTDUm_wkgbBiAiJN6UJaXTp3LrbO6WsYL2OeHZYA?accept=image/gif,image/jpeg,image/jpg,image/png,image/webp,audio/aac&sz=862)

## **Structure the project directory**

After defining the basics for my project and establishing the relationship between the models, it was time to structure the project directory. This is how my directory looks like:

![](https://keep.google.com/u/0/media/v2/19VqEMjEljTHrLUL-sYguUwLUpRu2LqI5ZsGoOBXCZ7fePQ11qTeAXnLMuzHh/1u9WL00HhaEBAoV-tUj0p1hrtC3dWI6yUI6WPa_bnF0l4iZwKTEWnyUHZBe0lFRw?accept=image/gif,image/jpeg,image/jpg,image/png,image/webp,audio/aac&sz=602)

![](https://keep.google.com/u/0/media/v2/1MMJEvsj3ibLdhqsqjeDRvXMi8yEoODUl-S-TH6ZaFTjPWJNJ2khMVmmLI6LMlgg/1F-XLfCIlP6qJm-ds7xYoAhsYEYLp9WdmNBWVYU2nSKcFAq4-LrgZRURZuZYmnHs?accept=image/gif,image/jpeg,image/jpg,image/png,image/webp,audio/aac&sz=267)


## **Setup Active Record in Sinatra**


**a)   Add your dependencies:**


 ```
gem 'sinatra'
 gem 'thin'
 gem 'require_all'
 gem 'activerecord', '5.2'
 gem 'sinatra-activerecord'
 gem 'rake'
 
 group :development do
   gem 'shotgun'
   gem 'pry'
   gem 'tux'
   gem 'sqlite3', '~> 1.3.6'
 End
```

These are the essential gems for the app to function with Active Record and Sinatra. We can always add more as we go depending on the functionality we want to include in the app.
	

**b)  Create your environment and connect to the database**

I configure my database connection in a .yml file, which is environment-aware. My configuration is as follow:

```
development:
  adapter: sqlite3
  encoding: unicode
  database: db/development.sqlite3
  pool: 5
```


**My environment.rb**

![](https://keep.google.com/u/0/media/v2/1GeOQxjIF7ZOuLIfECVh-ALpYqEN50kb33fOBBTs4pdFOtBgDiNmCMzVInpilVTs/1D3zLE3-EcC7gk6uFUasfDLFCgpJlKXQpvG57yZjcxH5hOtXjI1r7TqkC3y093A?accept=image/gif,image/jpeg,image/jpg,image/png,image/webp,audio/aac&sz=544)

**c)  Make a Rakefile in your root directory**

-
	Rake allows us to specify tasks and describe dependencies. We will need to load our environment file, as well as the “sinatra/activerecord/rake” gem to use rake tasks from the sinatra/activerecord library.
	
```
1   ENV["SINATRA_ENV"] ||= "development"
 
2   require './config/environment'
3   require 'sinatra/activerecord/rake'
4   
5   # to test models and database during development. run rake console
6   task :console do
7       Pry.start
8   end
```

We can** run rake -T** to view all the available rake tasks in our console.
Test the database connection, create migration by running **rake db:create_migration NAME=create_users **, where NAME is a parameter (users is the table name). This will create the db/migrate directory in your root directory. Code to create the table in the created migration file:

![](https://lh6.googleusercontent.com/94-h8JN9kxyMx8AbYiw6A4iuM2L6SJrZz1lwCGx6sRYHWUSacYAxSXL5Ik4tiZ4Jm4ZI_VEgYAOgnQ2vjXZ2w0sZr3OTPOX58_R-jZYjZGkiAUhCalWoWDaWbojVYKWsQY_cb5Ky)

**d) Create the corresponding models to your tables**

	
	
The model will inherit from ActiveRecord::Base class, therefore inheriting the magical active record methods to manipulate the database.

```
1	  class User < ActiveRecord::Base
2		    validates_presence_of :username, :email
3		    has_secure_password
4		    has_many :challenges
5		    has_many :types, through: :challenges
6	end
```


After creating your tables and corresponding models, don’t forget to run **rake db:migrate** to migrate the database.


**e)  Add config.ru to the root directory**


This is the most important file. It is a Rack configuration file. It is the bridge between the app and the world of the web. We first need to load our environment, then mount our controllers:

```
1   	require './config/environment'
2
3	   if ActiveRecord::Base.connection.migration_context.needs_migration?
4	 	     raise 'Migrations are pending. Run `rake db:migrate SINATRA_ENV=development` to resolve the issue.'
5		end
6
7	   # auto-add controllers
8	   use Rack::MethodOverride
9   Dir[File.join(File.dirname(__FILE__), "app/controllers", "*.rb")].collect {|file| File.basename(file).split(".")[0] }.reject {|file|    file == "application_controller" }.each do |file|
10		   string_class_name = file.split('_').collect { |w| w.capitalize }.join
11		   class_name = Object.const_get(string_class_name)
12		   use class_name
13	end
13	run ApplicationController
```


## Create Your Controllers & Views, Add Routes to Controllers and Style your Pages.

It’s finally time to create your web application. Set your controllers configurations:


```
class ApplicationController < Sinatra::Base
    register Sinatra::ActiveRecordExtension
    set :views, Proc.new { File.join(root, "../views/") }
    set :public_dir, "public"
end
```

Add your routes, add create their corresponding pages to display in the views folder; and last, be creative and style your pages as you please.

![](https://lh3.googleusercontent.com/vYKTmH-2g9FQN7jBc2RqA6Zifc7NZl2I73ms1CKAnwiF5nca1GYEeug2kcF_lrBVYgwvn5jbrBJGd47wKW4ROzNyf9jBUTZMMK826IdH7FWYcuzMzfqOTRmIYiSypP1a4k-ZljhG)

Check my code here: [](https://github.com/vanessuniq/get-fit-sinatra-app)

Keep up the momentum!!



