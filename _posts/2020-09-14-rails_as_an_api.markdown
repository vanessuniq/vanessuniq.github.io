---
layout: post
title:      "RAILS AS AN API"
date:       2020-09-14 14:44:27 -0400
permalink:  rails_as_an_api
---


![image](https://user-images.githubusercontent.com/46642178/93123441-1705ba00-f696-11ea-83de-b4ca6996c22f.png)


After working on several partially completed projects from the JavaScript module at Flatiron School, here came the time to finally build everything from scratch: a JavaScript (client-side) + Rails (server-side) application. In this blog, I am going to walk you through my Rails API building process. This is a basic Question and Answer engine for web dev. The user registration was not implemented for this project. The relationship is as follow:

![image](https://user-images.githubusercontent.com/46642178/93124752-1c640400-f698-11ea-9236-6a13f918156c.png)

**I.	Creating Rails API**

In the project directory, I typed the following in the terminal:

// ♥  rails new qa-engine-backend –api

This generated an MVC structure as well as other tools that allow us to work in an organized fashion way. The –api flag  is what make the app api only by removing a lot of default features, mostly browser related, that is not needed for structuring the api data to be rendered. My directory looked like this:

![image](https://user-images.githubusercontent.com/46642178/93061983-4e4a7b80-f642-11ea-9f26-373f498713f1.png)

The most essential folders, at least were we spent most of the time coding are:
MVC: this is the app model, view and Controller that favorizes the separation of concerns and implement our code design. 
Config/routes.rb : this is the location  for the application routes that will be available in our controllers
Db: the database folder that will contain all the migrations and our relational database.

**II.	Generating the Question and Answer Resources**

Rails generate resource allow us to do 4 steps in 1 by generating the models, controllers, views, migrations in one step, and adding the routes to config/routes.rb.  This save us some time in creating each individual file. 
*// ♥  rails g resource Question author body title topic*
*// ♥  rails g resource Answer author body question:references*  

In the above command, we declare the class name with its attributes, which will be string type. References is used to set the relationship between question and answer in the database. Notice it is set on the dependent model. When saving each instance of models in the database, it will create an id variable that will be unique to each instance/observation in each table.
Next, I customized the routes in the config/routes.rb to look as follow:

![image](https://user-images.githubusercontent.com/46642178/93108639-0d258c00-f681-11ea-95d1-32b4c2650806.png)

I only provided the create, index and delete action controller for my Question model, and the create, index, update and delete for the Answer model.
The generated migration for each model is as follow:

![image](https://user-images.githubusercontent.com/46642178/93108966-760d0400-f681-11ea-89e8-c873518718a2.png)
![image](https://user-images.githubusercontent.com/46642178/93109421-077c7600-f682-11ea-9990-3753011253e3.png)

With this in place, I ran *// ♥  rails db:migrate*  to migrate the database.

**III.	Filling Our MVC**

**a.	Models**

In the model, I added relationships and validations for each model to prevent creating instances of class with wrong data. It’s important to work on one model at the time and make sure that relationships align with the migrations. The results are below:

![image](https://user-images.githubusercontent.com/46642178/93111103-2da31580-f684-11ea-9ac9-b8dcd4f80b0e.png)
![image](https://user-images.githubusercontent.com/46642178/93111219-50cdc500-f684-11ea-8001-d3869913fcfb.png)

As seen in the Question model, I limited the topic to 4 selections.
We should always drop into the rails console (by running *// ♥  rails c* in the terminal). Test our models relationship and validations, and ensure that everything works as intended.

![image](https://user-images.githubusercontent.com/46642178/93112752-5cba8680-f686-11ea-8666-99b2c6ce15f9.png)

I also seeded the database with some dummy data  using the gem “faker” for further test of the app. Simply add the gem to the Gemfile, then run  /*/ ♥  bundle install *in the terminal and you will be set to use the gem. Check [https://github.com/faker-ruby/faker](http://) for more instruction on how to use the gem. 

**b.	View** 

This directory is not necessary, as no page needs to be rendered on the web. Our backend API will pass data to the front-end using json (JavaScript Object Notation) format. Json  is a human readable data interchange format used for representing data structures and objects in the browser. Rendering data in json format will be done by invoking render json at the bottom of the action controllers that are meant to submit data to the front-end side. Plus, I used a serialization to help turning rails object into json format. The serializer I used is called fast_jsonapi. To use it, add gem ‘fast_jsonapi’ to the Gemfile then bundle install in the terminal. Learn more about the gem at [https://github.com/Netflix/fast_jsonapi](http://) .
To create a serializer for each model,  run *// ♥  rail g serializer modelname* . This command will create a serializer folder in the project app directory, and include the model serializer file (app/serializer/model_serializer.rb.

![image](https://user-images.githubusercontent.com/46642178/93120299-246c7580-f691-11ea-930d-99fcd9c62497.png)

My serializer files look :

![image](https://user-images.githubusercontent.com/46642178/93120560-8c22c080-f691-11ea-969e-870ded6a05a1.png)
![image](https://user-images.githubusercontent.com/46642178/93120807-e3289580-f691-11ea-90c3-572aec219e82.png)

Notice how I specified which attributes I want to render to the outside world, as well as the relationship between the models. 

**c.	Controller**

Here is the place to add RESTful route actions.  in rails, this is referred as CRUD (CREATE, READ, UPDATE, DESTROY). The new and edit routes are left out as it is not needed for an API. Create is implemented by the create action controller, Read by index and show, Update by the update action controller and Destroy by the destroy action controller. It’s important to note that since this is an API, the applicationController inherits from ActiveRecord::API

![image](https://user-images.githubusercontent.com/46642178/93115480-e9b30f00-f689-11ea-8095-bc2f74311432.png)

After implementing all the conditions to safely handle errors, the final controllers for the models are as follow:

![image](https://user-images.githubusercontent.com/46642178/93116889-ecaeff00-f68b-11ea-85a5-bc427e9c93dc.png)
![image](https://user-images.githubusercontent.com/46642178/93117182-7068eb80-f68c-11ea-8749-3105cab00534.png)

I added the strong params for both Question and Answer as a private method to filter incoming data and ensure that the classes instances are created and updated appropriately.  Also check how I used the serializer to passed data to render json method.

**IV.	Wrapping Up and Fire our API**

With everything setup, we now have a server…

![image](https://user-images.githubusercontent.com/46642178/93122172-0c4a2580-f694-11ea-9413-8e47d6c34282.png)

It’s time to fire up the server by running *// ♥  rails s *  and hit one of our URLs that renders data.
Going to **localhost:3000/questions**, we get the following in the browser:

![image](https://user-images.githubusercontent.com/46642178/93123298-dd34b380-f695-11ea-8ae2-b10dcb737c72.png)

CHECK THE CODE REPOSITORY  <a href="https://github.com/vanessuniq/qa-engine/tree/master/qa-engine-backend">HERE</a>

