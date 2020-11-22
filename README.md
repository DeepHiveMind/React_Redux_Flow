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

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(2).png)

You can find step by step to implement these back-end servers in following tutorial:

* Spring Boot JWT with Spring Security (MySQL/PostgreSQL)
* Spring Boot JWT Authentication with Spring Security, MongoDB
* Node.js & JWT – Token Based Authentication & Authorization with MySQL
* Node.js JWT Authentication & Authorization with MongoDB

## React Component Diagram with Redux, Router, Axios

Look at the diagram below.

![](https://github.com/DeepHiveMind/React_Redux_Flow/blob/main/Images/Image1%20(2).png)


– The App page is a container with React Router. It gets app state from Redux Store. Then the navbar now can display based on the state.

– Login & Register pages have form for data submission (with support of react-validation library). They dispatch auth actions (login/register) to Redux Thunk Middleware which uses auth.service to call API.

– auth.service methods use axios to make HTTP requests. Its also store or get JWT from Browser Local Storage inside these methods.

– Home page is public for all visitor.

– Profile page displays user information after the login action is successful.

– BoardUser, BoardModerator, BoardAdmin pages will be displayed in navbar by state user.roles. In these pages, we use user.service to access protected resources from Web API.

– user.service uses auth-header() helper function to add JWT to HTTP header. auth-header() returns an object containing the JWT of the currently logged in user from Local Storage.








