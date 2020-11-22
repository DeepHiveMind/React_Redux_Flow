https://bezkoder.com/react-hooks-redux-login-registration-example/

# React_Redux_Flow
React_Redux flow for State Management 

## Agenda 
Build a React Redux Login, Logout, Registration example with LocalStorage, React Router, Axios and Bootstrap using React.js Hooks

- JWT Authentication Flow for User Registration & User Login, Logout
- Project Structure for React Redux JWT Authentication, LocalStorage, Router, Axios
- Working with Redux Actions, Reducers, Store for Application state
- Creating React Function Components with Hooks & Form Validation
- React Function Components for accessing protected Resources (Authorization)
- Dynamic Navigation Bar in React App

## Overview of React Redux Registration & Login example

Build a React.js application using Hooks:

* There are Login/Logout, Signup pages.
* Form data will be validated by front-end before being sent to back-end.
* Depending on User’s roles (admin, moderator, user), Navigation Bar changes its items automatically.

Screenshots: Registration Page:

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(1).png)

Signup Failed: 

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(9).png)

Form Validation Support:

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(8).png)

Login Page:

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(7).png)

Profile Page (for successful Login):

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(6).png)

For Moderator account login, the navigation bar will change by authorities:

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(5).png)

Check Browser Local Storage:

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(3).png)

Check State in Redux using redux-devtools-extension:

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(2).png)


## User Registration and User Login Flow

For JWT Authentication, we’re gonna call 2 endpoints:

* POST api/auth/signup for User Registration
* POST api/auth/signin for User Login

The following flow shows you an overview of Requests and Responses that React.js Client will make or receive. This React Client must add a JWT to HTTP Header before sending request to protected resources.

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Imag1%20(4).png)

You can find step by step to implement these back-end servers in following tutorial:

* Spring Boot JWT with Spring Security (MySQL/PostgreSQL)
* Spring Boot JWT Authentication with Spring Security, MongoDB
* Node.js & JWT – Token Based Authentication & Authorization with MySQL
* Node.js JWT Authentication & Authorization with MongoDB

## React Component Diagram with Redux, Router, Axios

Look at the diagram below.

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(3).png)


– The App page is a container with React Router. It gets app state from Redux Store. Then the navbar now can display based on the state.

– Login & Register pages have form for data submission (with support of react-validation library). They dispatch auth actions (login/register) to Redux Thunk Middleware which uses auth.service to call API.

– auth.service methods use axios to make HTTP requests. Its also store or get JWT from Browser Local Storage inside these methods.

– Home page is public for all visitor.

– Profile page displays user information after the login action is successful.

– BoardUser, BoardModerator, BoardAdmin pages will be displayed in navbar by state user.roles. In these pages, we use user.service to access protected resources from Web API.

– user.service uses auth-header() helper function to add JWT to HTTP header. auth-header() returns an object containing the JWT of the currently logged in user from Local Storage.

## Technology
The below modules are used:

* React 16
* react-redux 7.2.1
* redux 4.0.5
* redux-thunk 2.3.0
* react-router-dom 5
* axios 0.19.2
* react-validation 3.0.7
* Bootstrap 4
* validator 13.1.1

## Project Structure

The folders & files structure for this React Redux Registration and Login application:

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Imag1%20(2).png)

With the explanation in diagram above, you can understand the project structure easily.

But as to Redux elements that are used:
– actions folder contains all the action creators (auth.js for register & login, message.js for response message from server).
– reducers folder contains all the reducers, each reducer updates a different part of the application state corresponding to dispatched action.

## Setup React.js Project

Open cmd at the folder you want to save Project folder, run command:
  
    npx create-react-app react-redux-hooks-jwt-auth

Add Router Dom Module for later use: 

    npm install react-router-dom.

## Import Bootstrap
Run command: npm install bootstrap.

Open src/App.js and modify the code as following - 

    import React from "react";
    import "bootstrap/dist/css/bootstrap.min.css";

    const App = () => {
      // ...
    }

    export default App;

## Create Services

Create two services in src/services folder:

* Authentication service
* Data service

 * services

          auth-header.js

          auth.service.js (Authentication service)

          user.service.js (Data service)
    
Before working with these services, install Axios with command:
           npm install axios

## Authentication service

The service uses Axios for HTTP requests and Local Storage for user information & JWT.

It provides following important functions:

* register(): POST {username, email, password}
* login(): POST {username, password} & save JWT to Local Storage
* logout(): remove JWT from Local Storage

services/auth.service.js

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Imag1%20(1).png)

## Data service

In the case we access protected resources, the HTTP request needs Authorization header.

Create a helper function called authHeader() inside auth-header.js:

    export default function authHeader() {
      const user = JSON.parse(localStorage.getItem('user'));

      if (user && user.accessToken) {
        return { Authorization: 'Bearer ' + user.accessToken };
      } else {
        return {};
      }
    }

The code above checks Local Storage for user item. If there is a logged in user with accessToken (JWT), return HTTP Authorization header. Otherwise, return an empty object.

Note: For Node.js Express back-end, please use x-access-token header:

    export default function authHeader() {
      const user = JSON.parse(localStorage.getItem('user'));

      if (user && user.accessToken) {
        // for Node.js Express back-end
        return { 'x-access-token': user.accessToken };
      } else {
        return {};
      }
    }


Now define a service for accessing data in services/user.service.js:

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(2).png)

## Create Redux Actions

Create two kind of actions in src/actions folder:

 * actions

          types.js

          auth.js (register/login/logout actions)

          message.js (set/clear message actions)

## Action Types
Define some string constant that indicates the type of action being performed.

actions/type.js

    export const REGISTER_SUCCESS = "REGISTER_SUCCESS";
    export const REGISTER_FAIL = "REGISTER_FAIL";
    export const LOGIN_SUCCESS = "LOGIN_SUCCESS";
    export const LOGIN_FAIL = "LOGIN_FAIL";
    export const LOGOUT = "LOGOUT";

    export const SET_MESSAGE = "SET_MESSAGE";
    export const CLEAR_MESSAGE = "CLEAR_MESSAGE";

## Message Actions Creator

The Redux action creator is for actions related to messages (notifications) from APIs.

actions/message.js

    import { SET_MESSAGE, CLEAR_MESSAGE } from "./types";

    export const setMessage = (message) => ({
      type: SET_MESSAGE,
      payload: message,
    });

    export const clearMessage = () => ({
      type: CLEAR_MESSAGE,
    });

## Auth Actions Creator
This is creator for actions related to authentication. We’re gonna import AuthService to make asynchronous HTTP requests with trigger one or more dispatch in the result.

– register()

* calls the AuthService.register(username, email, password)
* dispatch REGISTER_SUCCESS and SET_MESSAGE if successful
* dispatch REGISTER_FAIL and SET_MESSAGE if failed

– login()

* calls the AuthService.login(username, password)
* dispatch LOGIN_SUCCESS and SET_MESSAGE if successful
* dispatch LOGIN_FAIL and SET_MESSAGE if failed

Both action creators return a Promise for Components using them.

actions/auth.js

    import {
      REGISTER_SUCCESS,
      REGISTER_FAIL,
      LOGIN_SUCCESS,
      LOGIN_FAIL,
      LOGOUT,
      SET_MESSAGE,
    } from "./types";

    import AuthService from "../services/auth.service";

    export const register = (username, email, password) => (dispatch) => {
      return AuthService.register(username, email, password).then(
        (response) => {
          dispatch({
            type: REGISTER_SUCCESS,
          });

          dispatch({
            type: SET_MESSAGE,
            payload: response.data.message,
          });

          return Promise.resolve();
        },
        (error) => {
          const message =
            (error.response &&
              error.response.data &&
              error.response.data.message) ||
            error.message ||
            error.toString();

          dispatch({
            type: REGISTER_FAIL,
          });

          dispatch({
            type: SET_MESSAGE,
            payload: message,
          });

          return Promise.reject();
        }
      );
    };

    export const login = (username, password) => (dispatch) => {
      return AuthService.login(username, password).then(
        (data) => {
          dispatch({
            type: LOGIN_SUCCESS,
            payload: { user: data },
          });

          return Promise.resolve();
        },
        (error) => {
          const message =
            (error.response &&
              error.response.data &&
              error.response.data.message) ||
            error.message ||
            error.toString();

          dispatch({
            type: LOGIN_FAIL,
          });

          dispatch({
            type: SET_MESSAGE,
            payload: message,
          });

          return Promise.reject();
        }
      );
    };

    export const logout = () => (dispatch) => {
      AuthService.logout();

      dispatch({
        type: LOGOUT,
      });
    };
    
## Create Redux Reducers

There will be two reducers in src/reducers folder, each reducer updates a different part of the state corresponding to dispatched Redux actions.



* reducers

     index.js

     auth.js (register/login/logout)

     message.js (set/clear message)

## Message Reducer
The reducer updates message state when message action is dispatched from anywhere in the application.

reducers/message.js

    import { SET_MESSAGE, CLEAR_MESSAGE } from "../actions/types";

    const initialState = {};

    export default function (state = initialState, action) {
      const { type, payload } = action;

      switch (type) {
        case SET_MESSAGE:
          return { message: payload };

        case CLEAR_MESSAGE:
          return { message: "" };

        default:
          return state;
      }
    }


## Auth Reducer
The Auth reducer will update the isLoggedIn and user state of the application.

reducers/auth.js

    import {
      REGISTER_SUCCESS,
      REGISTER_FAIL,
      LOGIN_SUCCESS,
      LOGIN_FAIL,
      LOGOUT,
    } from "../actions/types";

    const user = JSON.parse(localStorage.getItem("user"));

    const initialState = user
      ? { isLoggedIn: true, user }
      : { isLoggedIn: false, user: null };

    export default function (state = initialState, action) {
      const { type, payload } = action;

      switch (type) {
        case REGISTER_SUCCESS:
          return {
            ...state,
            isLoggedIn: false,
          };
        case REGISTER_FAIL:
          return {
            ...state,
            isLoggedIn: false,
          };
        case LOGIN_SUCCESS:
          return {
            ...state,
            isLoggedIn: true,
            user: payload.user,
          };
        case LOGIN_FAIL:
          return {
            ...state,
            isLoggedIn: false,
            user: null,
          };
        case LOGOUT:
          return {
            ...state,
            isLoggedIn: false,
            user: null,
          };
        default:
          return state;
      }
    }


## Combine Reducers

Because we only have a single store in a Redux application. Using reducer composition instead of many stores to split data handling logic.

reducers/index.js

    import { combineReducers } from "redux";
    import auth from "./auth";
    import message from "./message";

    export default combineReducers({
      auth,
      message,
    });
    
##   Create Redux Store
This Store will bring Actions and Reducers together and hold the Application state.

Now we need to install Redux, Thunk Middleware and Redux Devtool Extension.
Run the command: 

    npm install redux redux-thunk
    npm install --save-dev redux-devtools-extension
    
In the previous section, used combineReducers() to combine 2 reducers into one. Importing it, and pass it to createStore():

store.js    
    
    import { createStore, applyMiddleware } from "redux";
    import { composeWithDevTools } from "redux-devtools-extension";
    import thunk from "redux-thunk";
    import rootReducer from "./reducers";

    const middleware = [thunk];

    const store = createStore(
      rootReducer,
      composeWithDevTools(applyMiddleware(...middleware))
    );

    export default store;   
    
##  Create React Pages for Authentication

In src folder, create new folder named components and add several files as following:

* components

* Login.js

* Register.js

* Profile.js

## Form Validation overview

Now a library for Form validation, add react-validation library to our project.
Run the command: npm install react-validation validator

To use react-validation in this example, you need to import following items:

    import Form from "react-validation/build/form";
    import Input from "react-validation/build/input";
    import CheckButton from "react-validation/build/button";

    import { isEmail } from "validator";

Use isEmail() function from validator to verify email.

This is how we put them in render() method with validations attribute:

    const required = value => {
      if (!value) {
        return (
          <div className="alert alert-danger" role="alert">
            This field is required!
          </div>
        );
      }
    };

    const email = value => {
      if (!isEmail(value)) {
        return (
          <div className="alert alert-danger" role="alert">
            This is not a valid email.
          </div>
        );
      }
    };

    render() {
      return (
      ...
        <Form
          onSubmit={handleLogin}
          ref={form}
        >
          ...
          <Input
            type="text"
            className="form-control"
            ...
            validations={[required, email]}
          />

          <CheckButton
            style={{ display: "none" }}
            ref={checkBtn}
          />
        </Form>
      ...
      );
    }

Calling all methods to check Jesus validation functions in validations. Then CheckButton helps in verify if the form validation is successful or not. 

form.validateAll();

if (checkBtn.context._errors.length === 0) {
  // do something when no error
}

## Login Page

This page has a Form with username & password.
– Verify them as required field.
– If the verification is ok, we call AuthService.login() method, then direct user to Profile page: props.history.push("/profile");, or show message with response error.

For getting the application state and dispatching actions, we use React Redux Hooks useSelector and useDispatch.
– by checking isLoggedIn, we can redirect user to Profile page.
– message gives us response message.

components/Login.js

    import React, { useState, useRef } from "react";
    import { useDispatch, useSelector } from "react-redux";
    import { Redirect } from 'react-router-dom';

    import Form from "react-validation/build/form";
    import Input from "react-validation/build/input";
    import CheckButton from "react-validation/build/button";

    import { login } from "../actions/auth";

    const required = (value) => {
      if (!value) {
        return (
          <div className="alert alert-danger" role="alert">
            This field is required!
          </div>
        );
      }
    };

    const Login = (props) => {
      const form = useRef();
      const checkBtn = useRef();

      const [username, setUsername] = useState("");
      const [password, setPassword] = useState("");
      const [loading, setLoading] = useState(false);

      const { isLoggedIn } = useSelector(state => state.auth);
      const { message } = useSelector(state => state.message);

      const dispatch = useDispatch();

      const onChangeUsername = (e) => {
        const username = e.target.value;
        setUsername(username);
      };

      const onChangePassword = (e) => {
        const password = e.target.value;
        setPassword(password);
      };

      const handleLogin = (e) => {
        e.preventDefault();

        setLoading(true);

        form.current.validateAll();

        if (checkBtn.current.context._errors.length === 0) {
          dispatch(login(username, password))
            .then(() => {
              props.history.push("/profile");
              window.location.reload();
            })
            .catch(() => {
              setLoading(false);
            });
        } else {
          setLoading(false);
        }
      };

      if (isLoggedIn) {
        return <Redirect to="/profile" />;
      }

      return (
        <div className="col-md-12">
          <div className="card card-container">
            <img
              src="//ssl.gstatic.com/accounts/ui/avatar_2x.png"
              alt="profile-img"
              className="profile-img-card"
            />

            <Form onSubmit={handleLogin} ref={form}>
              <div className="form-group">
                <label htmlFor="username">Username</label>
                <Input
                  type="text"
                  className="form-control"
                  name="username"
                  value={username}
                  onChange={onChangeUsername}
                  validations={[required]}
                />
              </div>

              <div className="form-group">
                <label htmlFor="password">Password</label>
                <Input
                  type="password"
                  className="form-control"
                  name="password"
                  value={password}
                  onChange={onChangePassword}
                  validations={[required]}
                />
              </div>

              <div className="form-group">
                <button className="btn btn-primary btn-block" disabled={loading}>
                  {loading && (
                    <span className="spinner-border spinner-border-sm"></span>
                  )}
                  <span>Login</span>
                </button>
              </div>

              {message && (
                <div className="form-group">
                  <div className="alert alert-danger" role="alert">
                    {message}
                  </div>
                </div>
              )}
              <CheckButton style={{ display: "none" }} ref={checkBtn} />
            </Form>
          </div>
        </div>
      );
    };

    export default Login;


## Register Page
Similar to Login Page.

For Form Validation, there are some more details:

username: required, between 3 and 20 characters
email: required, email format
password: required, between 6 and 40 characters
We’re gonna dispatch register action and show response message (successful or error).

components/Register.js

import React, { useState, useRef } from "react";
import { useDispatch, useSelector } from "react-redux";

import Form from "react-validation/build/form";
import Input from "react-validation/build/input";
import CheckButton from "react-validation/build/button";
import { isEmail } from "validator";

import { register } from "../actions/auth";

const required = (value) => {
  if (!value) {
    return (
      <div className="alert alert-danger" role="alert">
        This field is required!
      </div>
    );
  }
};

const validEmail = (value) => {
  if (!isEmail(value)) {
    return (
      <div className="alert alert-danger" role="alert">
        This is not a valid email.
      </div>
    );
  }
};

const vusername = (value) => {
  if (value.length < 3 || value.length > 20) {
    return (
      <div className="alert alert-danger" role="alert">
        The username must be between 3 and 20 characters.
      </div>
    );
  }
};

const vpassword = (value) => {
  if (value.length < 6 || value.length > 40) {
    return (
      <div className="alert alert-danger" role="alert">
        The password must be between 6 and 40 characters.
      </div>
    );
  }
};

const Register = () => {
  const form = useRef();
  const checkBtn = useRef();

  const [username, setUsername] = useState("");
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [successful, setSuccessful] = useState(false);

  const { message } = useSelector(state => state.message);
  const dispatch = useDispatch();

  const onChangeUsername = (e) => {
    const username = e.target.value;
    setUsername(username);
  };

  const onChangeEmail = (e) => {
    const email = e.target.value;
    setEmail(email);
  };

  const onChangePassword = (e) => {
    const password = e.target.value;
    setPassword(password);
  };

  const handleRegister = (e) => {
    e.preventDefault();

    setSuccessful(false);

    form.current.validateAll();

    if (checkBtn.current.context._errors.length === 0) {
      dispatch(register(username, email, password))
        .then(() => {
          setSuccessful(true);
        })
        .catch(() => {
          setSuccessful(false);
        });
    }
  };

  return (
    <div className="col-md-12">
      <div className="card card-container">
        <img
          src="//ssl.gstatic.com/accounts/ui/avatar_2x.png"
          alt="profile-img"
          className="profile-img-card"
        />

        <Form onSubmit={handleRegister} ref={form}>
          {!successful && (
            <div>
              <div className="form-group">
                <label htmlFor="username">Username</label>
                <Input
                  type="text"
                  className="form-control"
                  name="username"
                  value={username}
                  onChange={onChangeUsername}
                  validations={[required, vusername]}
                />
              </div>

              <div className="form-group">
                <label htmlFor="email">Email</label>
                <Input
                  type="text"
                  className="form-control"
                  name="email"
                  value={email}
                  onChange={onChangeEmail}
                  validations={[required, validEmail]}
                />
              </div>

              <div className="form-group">
                <label htmlFor="password">Password</label>
                <Input
                  type="password"
                  className="form-control"
                  name="password"
                  value={password}
                  onChange={onChangePassword}
                  validations={[required, vpassword]}
                />
              </div>

              <div className="form-group">
                <button className="btn btn-primary btn-block">Sign Up</button>
              </div>
            </div>
          )}

          {message && (
            <div className="form-group">
              <div className={ successful ? "alert alert-success" : "alert alert-danger" } role="alert">
                {message}
              </div>
            </div>
          )}
          <CheckButton style={{ display: "none" }} ref={checkBtn} />
        </Form>
      </div>
    </div>
  );
};

      export default Register;
      Profile Page
      This page gets current User from Local Storage by getting user in the application state and show user information (with token).

      components/Profile.js

      import React from "react";
      import { Redirect } from 'react-router-dom';
      import { useSelector } from "react-redux";

      const Profile = () => {
        const { user: currentUser } = useSelector((state) => state.auth);

        if (!currentUser) {
          return <Redirect to="/login" />;
        }

        return (
          <div className="container">
            <header className="jumbotron">
              <h3>
                <strong>{currentUser.username}</strong> Profile
              </h3>
            </header>
            <p>
              <strong>Token:</strong> {currentUser.accessToken.substring(0, 20)} ...{" "}
              {currentUser.accessToken.substr(currentUser.accessToken.length - 20)}
            </p>
            <p>
              <strong>Id:</strong> {currentUser.id}
            </p>
            <p>
              <strong>Email:</strong> {currentUser.email}
            </p>
            <strong>Authorities:</strong>
            <ul>
              {currentUser.roles &&
                currentUser.roles.map((role, index) => <li key={index}>{role}</li>)}
            </ul>
          </div>
        );
      };

      export default Profile;

## Profile Page
The page gets current User from Local Storage by getting user in the application state and show user information (with token).

components/Profile.js

    import React from "react";
    import { Redirect } from 'react-router-dom';
    import { useSelector } from "react-redux";

    const Profile = () => {
      const { user: currentUser } = useSelector((state) => state.auth);

      if (!currentUser) {
        return <Redirect to="/login" />;
      }

      return (
        <div className="container">
          <header className="jumbotron">
            <h3>
              <strong>{currentUser.username}</strong> Profile
            </h3>
          </header>
          <p>
            <strong>Token:</strong> {currentUser.accessToken.substring(0, 20)} ...{" "}
            {currentUser.accessToken.substr(currentUser.accessToken.length - 20)}
          </p>
          <p>
            <strong>Id:</strong> {currentUser.id}
          </p>
          <p>
            <strong>Email:</strong> {currentUser.email}
          </p>
          <strong>Authorities:</strong>
          <ul>
            {currentUser.roles &&
              currentUser.roles.map((role, index) => <li key={index}>{role}</li>)}
          </ul>
        </div>
      );
    };

    export default Profile;

## Create React Pages for accessing Resources
The pages will use UserService to request data from API.

 * components

       Home.js

       BoardUser.js

       BoardModerator.js

       BoardAdmin.js
    
## Home Page
This is a public page that shows public content. People don’t need to log in to view this page.

components/Home.js   
    
    import React, { useState, useEffect } from "react";

    import UserService from "../services/user.service";

    const Home = () => {
      const [content, setContent] = useState("");

      useEffect(() => {
        UserService.getPublicContent().then(
          (response) => {
            setContent(response.data);
          },
          (error) => {
            const _content =
              (error.response && error.response.data) ||
              error.message ||
              error.toString();

            setContent(_content);
          }
        );
      }, []);

      return (
        <div className="container">
          <header className="jumbotron">
            <h3>{content}</h3>
          </header>
        </div>
      );
    };

    export default Home;    
    
## Role-based Pages
Have3 3 pages for accessing protected data:

* BoardUser page calls UserService.getUserBoard()
* BoardModerator page calls UserService.getModeratorBoard()
* BoardAdmin page calls UserService.getAdminBoard()

I will show you User Page for example, other Pages are similar to this Page.

components/BoardUser.js    
    
    import React, { useState, useEffect } from "react";

    import UserService from "../services/user.service";

    const BoardUser = () => {
      const [content, setContent] = useState("");

      useEffect(() => {
        UserService.getUserBoard().then(
          (response) => {
            setContent(response.data);
          },
          (error) => {
            const _content =
              (error.response &&
                error.response.data &&
                error.response.data.message) ||
              error.message ||
              error.toString();

            setContent(_content);
          }
        );
      }, []);

      return (
        <div className="container">
          <header className="jumbotron">
            <h3>{content}</h3>
          </header>
        </div>
      );
    };

    export default BoardUser;
    
## Add Navbar and define Routes

Create React Router History
This is a custom history object used by the React Router.

helpers/history.js   
    
    import { createBrowserHistory } from "history";

    export const history = createBrowserHistory();    


Modify App Page
Add a navigation bar in App Page. This is the root container for our application.
The navbar dynamically changes by login status and current User’s roles.

* Home: always
* Login & Sign Up: if user hasn’t signed in yet
* User: there is user value in the application state
* Board Moderator: roles includes ROLE_MODERATOR
* Board Admin: roles includes ROLE_ADMIN

src/App.js

      import React, { useState, useEffect } from "react";
      import { useDispatch, useSelector } from "react-redux";
      import { Router, Switch, Route, Link } from "react-router-dom";

      import "bootstrap/dist/css/bootstrap.min.css";
      import "./App.css";

      import Login from "./components/Login";
      import Register from "./components/Register";
      import Home from "./components/Home";
      import Profile from "./components/Profile";
      import BoardUser from "./components/BoardUser";
      import BoardModerator from "./components/BoardModerator";
      import BoardAdmin from "./components/BoardAdmin";

      import { logout } from "./actions/auth";
      import { clearMessage } from "./actions/message";

      import { history } from "./helpers/history";

      const App = () => {
        const [showModeratorBoard, setShowModeratorBoard] = useState(false);
        const [showAdminBoard, setShowAdminBoard] = useState(false);

        const { user: currentUser } = useSelector((state) => state.auth);
        const dispatch = useDispatch();

        useEffect(() => {
          history.listen((location) => {
            dispatch(clearMessage()); // clear message when changing location
          });
        }, [dispatch]);

        useEffect(() => {
          if (currentUser) {
            setShowModeratorBoard(currentUser.roles.includes("ROLE_MODERATOR"));
            setShowAdminBoard(currentUser.roles.includes("ROLE_ADMIN"));
          }
        }, [currentUser]);

        const logOut = () => {
          dispatch(logout());
        };

        return (
          <Router history={history}>
            <div>
              <nav className="navbar navbar-expand navbar-dark bg-dark">
                <Link to={"/"} className="navbar-brand">
                  bezKoder
                </Link>
                <div className="navbar-nav mr-auto">
                  <li className="nav-item">
                    <Link to={"/home"} className="nav-link">
                      Home
                    </Link>
                  </li>

                  {showModeratorBoard && (
                    <li className="nav-item">
                      <Link to={"/mod"} className="nav-link">
                        Moderator Board
                      </Link>
                    </li>
                  )}

                  {showAdminBoard && (
                    <li className="nav-item">
                      <Link to={"/admin"} className="nav-link">
                        Admin Board
                      </Link>
                    </li>
                  )}

                  {currentUser && (
                    <li className="nav-item">
                      <Link to={"/user"} className="nav-link">
                        User
                      </Link>
                    </li>
                  )}
                </div>

                {currentUser ? (
                  <div className="navbar-nav ml-auto">
                    <li className="nav-item">
                      <Link to={"/profile"} className="nav-link">
                        {currentUser.username}
                      </Link>
                    </li>
                    <li className="nav-item">
                      <a href="/login" className="nav-link" onClick={logOut}>
                        LogOut
                      </a>
                    </li>
                  </div>
                ) : (
                  <div className="navbar-nav ml-auto">
                    <li className="nav-item">
                      <Link to={"/login"} className="nav-link">
                        Login
                      </Link>
                    </li>

                    <li className="nav-item">
                      <Link to={"/register"} className="nav-link">
                        Sign Up
                      </Link>
                    </li>
                  </div>
                )}
              </nav>

              <div className="container mt-3">
                <Switch>
                  <Route exact path={["/", "/home"]} component={Home} />
                  <Route exact path="/login" component={Login} />
                  <Route exact path="/register" component={Register} />
                  <Route exact path="/profile" component={Profile} />
                  <Route path="/user" component={BoardUser} />
                  <Route path="/mod" component={BoardModerator} />
                  <Route path="/admin" component={BoardAdmin} />
                </Switch>
              </div>
            </div>
          </Router>
        );
      };

      export default App;

## Add CSS style for React Pages

Open src/App.css and write some CSS code as following:

    label {
      display: block;
      margin-top: 10px;
    }

    .card-container.card {
      max-width: 350px !important;
      padding: 40px 40px;
    }

    .card {
      background-color: #f7f7f7;
      padding: 20px 25px 30px;
      margin: 0 auto 25px;
      margin-top: 50px;
      -moz-border-radius: 2px;
      -webkit-border-radius: 2px;
      border-radius: 2px;
      -moz-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
      -webkit-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
      box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
    }

    .profile-img-card {
      width: 96px;
      height: 96px;
      margin: 0 auto 10px;
      display: block;
      -moz-border-radius: 50%;
      -webkit-border-radius: 50%;
      border-radius: 50%;
    }
