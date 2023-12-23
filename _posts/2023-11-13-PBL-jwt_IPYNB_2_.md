---
layout: post
toc: True
title: JWT Project
description: A Python Project that is secured with JSON Web Tokens (JWT)
author: Nikhil Chakravarthula
categories: ['C7.0']
type: ccc
permalink: /cpt/jwt
---

# JWT Project

## Link for Repository's
 - [Frontend](https://github.com/nikhilc3/Nighthawk-guessr)
 - [Backend](https://github.com/DavidVasilev1/flask3)
<hr>

## Frontend

### Login.html ( [Login.html](https://github.com/nikhilc3/Nighthawk-guessr/blob/gh-pages/login.html) )

The Login.html has a lot of styling and html due to there being a fucntional register and login in one html file. Let's Break it down:

- <b> HTML Section </b>: This part of the file is responsible for setting up the structure of the webpage. The HTML section of this file starts from the <!DOCTYPE html> declaration and extends till the closing </body> tag.

  - It defines two main sections, Login and SignUp, enclosed within div tags with id attributes set as 'login' and 'signup' respectively.
Each of these sections includes a combination of images and input fields to make up the form for user login and signup.
  - Input fields for username and password are provided along with corresponding images for better user experience.
  
<br>

<br>

- <b> JavaScript Section </b>: This section is responsible for the functionality of the webpage, i.e., what happens when a user interacts with the page. Lets Break it down:
  - It includes functions to handle user authentication with the auth function, user registration (createUser function), and switching between login and signup views (switchPage function).
  - The auth function makes a POST request to a the backend deployed URL which has all the endpoints, with the user's entered username and password. If the authentication is successful, the user is redirected to '/index.html' succesfully, with a token.
  - The createUser function similarly makes a POST request to create a new user when a user registers, and if successful, the user is logged in automatically, and the log in is saved for future uses.
  - There are also a number of functions responsible for adjusting the layout based on the window size for a responsive design and switching between the login and register on the same page.

- The include env.js which will inject the content of env.js at that point in the HTML file before the server sends it to the client. This is a way to provide dynamic content in the HTML file before it is sent to the client. 
- Env.js Contains: const api = ['localhost', '127.0.0.1'].includes(window.location.hostname) ? "http://localhost:8570" : "https://nighthawkguessr.duckdns.org"; and this is used with the fetch function to fetch from either local host or from depoloyed backend link. This is a very crucial part of the code

<br>
<br>
<br>

### Index html

This page is the home page of the website and is locked untill you login succesfully and have a token. Javascript Part is the only thing playing a role in JWT in the index.html so here it is:

- JavaScript Section
  - JavaScript functions are defined to handle various interactions on the page, such as authenticating a login, signing out, updating difficulty, redirecting to the game page, and handling UI changes when a difficulty option is selected.
  - An asynchronous function authenticateLogin fetches the user's authentication status from the server and makes the webpage content visible only if  the user is authenticated. If the authentication fails, the user is redirected to the login page.
  - The signOut function clears all cookies and reloads the page, effectively signing the user out.
  - Other functions like updateDifficulty, resetScoreCookie, and gameRedirect are used to manage the game's difficulty level and user scores.
  - The resize function adjusts the positions, text sizes, and image sizes of various elements on the page depending on the window size, ensuring that the webpage is responsive to different screen sizes.





- The nclude env.js which will inject the content of env.js at that point in the HTML file before the server sends it to the client. This is a way to provide dynamic content in the HTML file before it is sent to the client. 
- Env.js Contains: const api = ['localhost', '127.0.0.1'].includes(window.location.hostname) ? "http://localhost:8570" : "https://nighthawkguessr.duckdns.org"; and this is used with the fetch function to fetch from either local host or from depoloyed backend link. This is a crucial part of the code

<hr>

## Backend

### Jwt File (JWT_Auth.py)
(Most Important File)

The jwt_auth.py script is a part of my backend application that is responsible for managing JSON Web Tokens (JWTs), which are used for user authentication. This module provides functions for user registration and authentication, and also includes a route that provides information on the current user's authentication status.

- breakdown of the different components of the jwt_auth.py script:

  - Authentication Function: The authenticate(username, password) function is used to authenticate users. This function looks in the database for a user with the given username, and if such a user is found, it checks if the provided password, when hashed, matches the stored hashed password for that user. If the password is correct, the function returns the User object; otherwise, it returns None.

  - Identity Function: The identity(payload) function retrieves a user based on the identity payload of a JWT. This payload contains a user's ID, and the function queries the database to find and return the user with this ID.

- JWT Routes: routes under the jwt_auth blueprint: 

  - /auth_status: This GET route returns the username of the currently authenticated user, verifying the functionality of the JWT.

  - /register: This POST route registers a new user. It first validates the request data to ensure a username and password are provided. Then, it checks the database to make sure a user with the provided username doesn't already exist. If the username is available, it hashes the password using bcrypt, creates a new User object, adds it to the database session, commits the session to save the new user in the database, and finally sends a response indicating that the new user was successfully created.

### Creating Database File (DB_Create.py   &   User.py)

- db_create.py:

  - The db_create.py script is responsible for setting up the database structure for my application. It's one of the first steps taken in the lifecycle of my application, even before you can start registering users or performing any other actions. The crucial functionality it performs is calling db.create_all(), which is an SQLAlchemy command that looks at all of the classes that inherit from db.Model and creates corresponding tables in the database.

  - In the context of JWT registration, db_create.py is the script that creates the user table in the database, where user data will be stored. Without this table, you would have no place to store or retrieve user details, so user registration (and subsequent authentication) would not be possible.

- Let's now move to user.py:

  - user.py file defines the User model class. This class is a representation of the user table in the database. Each instance of this class corresponds to a row in the user table. It consists of three columns - id, username, and password.

  - id is a unique identifier for each user.
  - username is a string that stores the username of a user. It has been set as non-nullable and unique which means each user must have a username and that username must be unique.
  - password is a string that stores the hashed password of a user. It's non-nullable meaning every user must have a password. 
  - When a new user registers, the register() function in jwt_auth.py creates a new instance of the User class, filling in the username and hashed      password provided by the user. It then adds this new user to the database.

In the authentication process, the User model is used to look in the database for a user with a specific username or ID. This is essential in both the authenticate() function, which checks the submitted password against the stored hashed password for a username, and the identity() function, which retrieves a user based on their ID as stored in the JWT.

### Init.py Files

The init.py files in my application play an essential role in managing and facilitating dependencies among the modules.

- nighthawkguessr_api/init.py: This file mainly sets up the Flask application and its dependencies. In the context of JWT registration, it creates an instance of Flask and configures it to use a SQLite database. Additionally, it sets a secret key, which is critical for creating and verifying JWTs, and it prepares the SQLAlchemy object (db), which manages database interactions. The db object is later imported in the user model (User) and in the authentication routes (jwt_auth.py) to query and persist user information in the database.

- nighthawkguessr_api/model/init.py: This file is essentially the entry point for my models, and its primary role is to make the importing of different models simpler and more streamlined. In other words, instead of importing models individually from each file, you can import all at once from the model package. This is useful when multiple parts of the application need to access these models. Regarding JWT registration, the User model imported in this file is critical, as it is used in the jwt_auth.py file to handle user data during registration and authentication.



### Main.py

- In my Flask application, the main.py script plays a crucial role in JWT (JSON Web Token) authentication. JWT is a compact, self-contained method for securely transmitting information between parties as a JSON object. The information is verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.

- In the application, the jwt_auth.py file defines the authenticate and identity methods which are integral to the JWT authentication process. These two functions are utilized in main.py to set up JWT for the Flask app.

  - The authenticate function, defined in jwt_auth.py, takes in a username and password as parameters and authenticates the user. It checks if the user exists in the database and whether the provided password matches the stored (hashed) password for the user. If the credentials are correct, it returns the user object. This function is used during the generation of a JWT when a user logs in.

  - The identity function, also defined in jwt_auth.py, takes the payload of a JWT as a parameter and extracts the user id from it. It then retrieves the user associated with this id from the database and returns the user object. This function is used to get the current user (who claims to own the token) when a JWT is provided in a request to a protected endpoint.

- In the main.py script, these two functions are given as arguments to the JWT constructor, establishing a JWT object:

  - jwt = JWT(app, jwt_auth.authenticate, jwt_auth.identity)
    This line is crucial as it sets up JWT handling in my Flask app. It uses jwt_auth.authenticate for authenticating users when they log in, and jwt_auth.identity for identifying users from incoming JWTs in requests to protected routes.

  - Another crucial part of the main.py is 
    cors = CORS(app, origins=re.compile(".*"), supports_credentials=True)
    This line enables CORS and the supports_credentials = True is extremely important because it allows cookies or authenticated requests to be made cross origins.



### Requirements.txt 

-e file:./vendor/flask-jwt#egg=Flask-JWT

This line in requirements.txt is installing the Flask-JWT package from a local directory (./vendor/flask-jwt) in editable mode. This way, I am not dependent on the version available on PyPI or any changes made by the maintainers of the original package.

<hr>

## AWS Deployment

### Nginx Changes

<img width="1438" alt="Screenshot 2023-06-08 at 7 35 16 PM" src="https://github.com/nikhilc3/Blog1/assets/111464993/0c9e4dc2-9337-4b3b-8f8d-d2e3c28d87fe">


<hr>
