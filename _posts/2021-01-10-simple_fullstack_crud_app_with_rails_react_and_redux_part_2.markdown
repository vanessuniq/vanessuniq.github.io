---
layout: post
title:      "Simple Fullstack CRUD App With Rails, React &Redux Part 2"
date:       2021-01-10 02:53:58 -0500
permalink:  simple_fullstack_crud_app_with_rails_react_and_redux_part_2
---

<img src="https://i.morioh.com/d66b3cebfc.png"/>

## Introduction

In <a href="https://vanessuniq.medium.com/simple-fullstack-crud-app-with-rails-react-redux-54a9d49d8ac3" style="color:purple " >Part 1: User Authentication in Rails <a> 
we covered one basic way to implement user registration, login and logout functionalities in the server (Rails API) with JWT. 

Now we will take a look at how to register/login and logout users from our react app with JWT. 

Our React app pulls data from a Rails 5 API that serves JSON data on users info and post (user’s avatar, bio, username, questions and answers’ post and posts’ reactions).

The code for this React App can be find <a href="https://github.com/vanessuniq/UnitedWebDev" style="color:purple " >here <a>

## General Idea behind Auth with JWT in React & Redux

The main idea is  that the user will submit his/her credentials (username/ password) by filling out the registration or login form. The app will then send the credentials to our Rails server for authentication. If the server is able to authenticate the user, it will generate a unique token (JSON Web Token) associated with the user and forward it to the React app as well with the current user information. Once our  react app receives the JWT and current user, it will store the JWT to the local storage for easy retrieval to make subsequent authorized requests to the API, and the current user’s info to the Redux store for easy access across your app.

The Rails API  has the following routes for users:

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

**1. User Registration** <br/>
As a refresher, here is the flow for registration:

* a new user fill out the sign up form on the client side with valid username and password and submit the form
* a POST request is made to /api/v1/users which is redirected to the create action of the User Controller in the Rails API
* if an instance of user is successfully created, the server issues an encoded token to the client (React app), and send the user object as well
* the client then save the token into the localestorage and presents it with every request to authenticate the user on the server side, and save the current user in the Redux store

As we use Redux for our state management, do not forget to add `redux` and `react-redux` to your dependencies. `yarn add redux react-redux`
Our signup form is a controlled class component. Once the form is submitted, the app will run a fetch  post request to the API in the `userAction.js` file, making use of Redux’ thunk for the asynchronous request (make sure you have the thunk middleware installed).  A simple signup form component  looks like this:

```js
import React, {Component} from 'react';
import {connect} from 'react-redux';
import {register} from '../actions/userActions';
class Signup extends Component {
  state = {
    username: "",
    password: "",
    avatar: "",
    bio: ""
  }
handleChange = event => {
    this.setState({
      [event.target.name]: event.target.value
    });
  }
 
  handleSubmit = event => {
    event.preventDefault()
    this.props.register(this.state)
  }
 
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <h1>Welcome proactive learner, please sign up below</h1>
 
        <label>Username</label>
        <input
          name='username'
          placeholder='Username'
          value={this.state.username}
          onChange={this.handleChange}
          /><br/>
 
        <label>Password</label>
        <input
          type='password'
          name='password'
          placeholder='Password'
          value={this.state.password}
          onChange={this.handleChange}
          /><br/>
 
        <label>Avatar</label>
          <input
            name='avatar'
            placeholder='Avatar (URL)'
            value={this.state.avatar}
            onChange={this.handleChange}
            /><br/>
 
          <label>Bio</label>
          <textarea
            name='bio'
            placeholder='Bio'
            value={this.state.bio}
            onChange={this.handleChange}
            /><br/>
 
        <input type='submit'/>
      </form>
    )
  }
}
 
const mapDispatchToProps = dispatch => ({
  register: userInfo => dispatch(register(userInfo))
})
 
export default connect(null, mapDispatchToProps)(Signup);
 
```

The imported `register` function from userActions.js is the redux thunk that uses fetch to send a POST request to our Rails register endpoint at http://localhost:3000/api/v1/users. Upon form submission, the signup component dispatches the register action; thus the use of `mapDispatchToProps`.
The register function is define below:

```js
export const register = user => {
  return dispatch => {
    return fetch("http://localhost:3000/api/v1/users", {
      method: "POST",
      headers: {
        'Content-Type': 'application/json',
        Accept: 'application/json',
      },
      body: JSON.stringify({user})
    })
      .then(resp => resp.json())
      .then(data => {
        if (data.errors) {
          // Design the logic to handle invalid user creation.
        } else {
          localStorage.setItem("token", data.jwt)
          dispatch(loginUser(data.user))
        }
      })
  }
}
 
const loginUser = userObj => ({
    type: 'LOGIN_USER',
    payload: userObj
})
```

When Redux Promise resolves the Promise in our register function, the expected response is either a json object containing the user and the token  if the user creation was successful or an error object if not.

Successful user creation:

```ruby
render json: {user: UserSerializer.new(user), jwt: token}
```

Upon success, we store the jwt in the browser’s local storage for the session to persist using the `setItem` method, then we dispatch the login user action to the user reducer, passing the user object as the payload to be saved in the Redux store.

Here is a sample of the user reducer handling the dispatch of the `LOGIN_USER` action
userReducer.js

```js
const initialState = {
  all: [],
  currentUser: {},
}

export default function userReducer(state = initialState, action) {
    switch (action.type) {
      case 'LOGIN_USER':
       const newUser = state.all.find(user => user.id === action.payload.id)
            if (newUser) {
                return {...state, currentUser: action.payload }
            } else {
                return {...state, all: state.all.concat(action.payload), currentUser:action.payload}
            }; 
      default:
        return state;
    }
  }
```
When handling the LOGIN_USER action, the reducer first checks if the user already exists in the list of users in the store (this is to differentiate a registration followed by the login action to the simple login action). If it is a new registration, meaning `newUser` will evaluate to false in the if condition, the reducer will first add the new user to the list then set the user as the current user. Otherwise, the reducer will simply set the user object in the payload as the current user.

<p style="color:brown ">Note: In our case, we are automatically logging in the user after registering. We can also opt out and require the use to login after registration by using flash messaging and redirecting the user to the login page after successful registration.<p>

**2. User Login** <br/>

User login and signup have similar functionalities. Here is the sample from the Login component:

```jsx
import React, {Component} from 'react';
import {connect} from 'react-redux';
import {login} from  '../actions/userActions'';

class Login extends Component {
  state = {
    username: "",
    password: ""
  }

  handleChange = event => {
    this.setState({
      [event.target.name]: event.target.value
    });
  }

  handleSubmit = event => {
    event.preventDefault()
    this.props.login(this.state)
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <h1>Welcome back! Login to gain access</h1>

        <label>Username</label>
        <input
          name='username'
          placeholder='Username'
          value={this.state.username}
          onChange={this.handleChange}
          /><br/>

        <label>Password</label>
        <input
          type='password'
          name='password'
          placeholder='Password'
          value={this.state.password}
          onChange={this.handleChange}
          /><br/>

        <input type='submit'/>
      </form>
    )
  }
}

const mapDispatchToProps = dispatch => ({
  login: userInfo => dispatch(login(userInfo))
})

export default connect(null, mapDispatchToProps)(Login);

```

Here, the user submits the his/her login credentials by submitting the form. The submission event triggers the dispatch of the `login thunk` that will send a post request to the server (/api/v1/login endpoint). The server will then authenticate the user credentials and generate a JWT if validated, then send an object containing the JWT and user back to the client as seen in the sign up process. As seen before, the JWT will be stored in the browser local storage and the `LOGIN_USER` action will be dispatched to the user reducer to set the current user in the Redux store.

Here is the code for the login thunk in userActions.js:

```js

export const login = user => {
  return dispatch => {
    return fetch("http://localhost:3000/api/v1/login", {
      method: "POST",
      headers: {
        'Content-Type': 'application/json',
        Accept: 'application/json',
      },
      body: JSON.stringify({user})
    })
      .then(resp => resp.json())
      .then(data => {
        if (data.errors) {
          // logic to handle invalid login credentials and display the error on login form.
        } else {
          localStorage.setItem("token", data.jwt)
          dispatch(loginUser(data.user))
        }
      })
  }
}

```

**3. User Auto Login** <br/>

We set up the locale storage to persist a user session for when he/she revisits the page and wants to submit authorized requests to the server. So it is logical to try to auto login the user once the app loads and everytime it is accessed. To do so, we will dispatch the getProfile thunk  in the `componentDidMount` method of our App component. Once dispatched, the `getProfile` thunk will first check if there’s an existing token stored in the locale storage. If the token is present, it will run `fetch` sending a get request to the server (/api/v1/auto_login  endpoint) with an <p style="color:brown ">Authorization header</p> carrying the token. The Rails server will receive the token and attempt to decode it.  If it can successfully decode the token, it will retrieve the associated user from the database and send it back to the client as the response, otherwise it will send an error message.  
This is the sample code for the App component: 

```jsx

import React, { Component } from 'react';
import { Switch, Route } from 'react-router-dom';
import {connect} from 'react-redux';
import {getProfile} from './actions/userActions';
import Signup from './features/users/SignUp' ;
import Login from'./features/users/Login' ;


class App extends Component {
  componentDidMount = () => {
    this.props.getProfile()
  }

  render() {
    return (
      <div>
        <Switch>
          <Route path="/signup" component={Signup}/>
          <Route path="/login" component={Login}/>
        </Switch>
      </div>
    );
  }
}

const mapDispatchToProps = dispatch => ({
  getProfile: () => dispatch(getProfile())
})

export default connect(null, mapDispatchToProps)(App);

```

Here is the sample code for the `getProfile` thunk in userActions.js:

```js
export const getProfile = () => {
  return dispatch => {
    const token = localStorage.token;
    if (token) {
      return fetch("http://localhost:3000/api/v1/profile", {
        method: "GET",
        headers: {
          'Content-Type': 'application/json',
          Accept: 'application/json',
          'Authorization': `Bearer ${token}`
        }
      })
        .then(resp => resp.json())
        .then(data => {
          if (data.message) {
            //  remove the invalid token if the server returns an error message.
            localStorage.removeItem("token")
          } else {
            dispatch(loginUser(data.user))
          }
        })
    }
  }
}

```

As we see above, if the server returns an error message, we will want to remove the invalid token from the locale storage. If a valid user is returned, we will dispatch the LOGIN_USER action to the user reducer  that will set up the current user in our Redux store.
At this point, We should have a fully functioning authentication system. Give yourself a Have five if you made it this far.  Don’t forget you can track the Redux state and actions using the Redux DevTools if installed in your chrome browser.

**4. User Logout** <br/>

We can create a button that gives the users the ability to logout and terminate their session. That button should be placed somewhere easily accessible to the user. Thus, the best place to house our logout button will be in our navigation bar/ header.

A simple code implementation could be as follow:

```jsx

import React, { Component } from 'react';
import {logoutUser } from './actions/userActions';
import { Link } from 'react-router-dom'

class Nav extends Component {
	handleClick = event => {
    event.preventDefault()
    // Remove the token from localStorage
    localStorage.removeItem("token")
    // Remove the user object from the Redux store
    this.props.logoutUser()
  	}

	render() {
	Const auth = (<>
				<Link to=’/login’> Login</Link>
				<Link to=’/signup’> Sign up </Link>
			</>)
		return (
				<nav>
					{this.props.currentUser.username? 
<button onClick={this.handleClick}>
Log Out
</button>
  : auth
 }

				</nav>
			)
}
}

const mapStateToProps = state => ({
  currentUser: state.users.currentUser
})

const mapDispatchToProps = dispatch => ({
    logoutUser: () => dispatch(logoutUser())
})

export default connect(mapStateToProps, mapDispatchToProps)(Nav);

```

In the above code, the navbar displays different features depending on whether there’s a current user logged in or not. If there’s a current user, the logout button will be displayed; otherwise the login and sign up buttons are displayed.
`mapStateToProps` is  being used in order for the Nav component to receive a prop called currentUser from the store.
Once the user clicks on the logout button, it will trigger the token to be removed from the local storage, and the LOGOUT_USER action to be dispatched to the user reducer, which will set the global state current user to an empty object.

Here is the code for the `logoutUser` action creator function in userActions.js:

```js

export const logoutUser = () => ({
  type: 'LOGOUT_USER'
})

```
Here is the updated user reducer:

```js

const initialState = {
  all: [],
  currentUser: {},
}

export default function userReducer(state = initialState, action) {
    switch (action.type) {
      case 'LOGIN_USER':
       const newUser = state.all.find(user => user.id === action.payload.id)
            if (newUser) {
                return {...state, currentUser: action.payload }
            } else {
                return {...state, all: state.all.concat(action.payload), currentUser:action.payload}
            }; 
	case 'LOGOUT_USER':
        	    return {...state, currentUser: {} }
      default:
        return state;
    }
  }

```

This is it for the basic implementation of JWT in  React & Redux.  Please read  more about JWT and other approaches  <a href='https://jwt.io/introduction/'>here</a>.

In the last part of this guide (part 3), we will cover how to create, read , update and delete a question and how that process is synced between the back end and the front end.

Until then, Happy Coding!!




