---
toc: False
layout: post
hide: False
title: P2 | Python/Flask
description: Team Teach On Python/Flask
courses: {'compsci': {'week': 15}}
type: tangibles
---

## Flask & SQLAlchemy!

### Overview
* SQLAlchemy: Python tool for interacting with SQL databases
    * “Pythonic” way of interacting with databases
    * Flask-SQLAlchemy: Extension for Flask, bridging Python and SQL databases

## Setup to install Flask-SQLAlchemy

* Use pip to install Flask-SQLAlchemy in your virtual environment
    * SQLAlchemy supports different SQL database systems like mySQL. Additional modules might be required for specific databases

## Basics of using a database with Flask

* Establish a connection between your Flask app and an existing SQL database
    * Understand database username, password, and connection details

    * Username and password for authentication
    * Connection specifies the location and characteristics of the database server, including its address and port number.

    * learn Flask-SQLAlchemy syntax for reading data

    Go over later in example

## Database Interaction Scenarios

* Reading from the database - retrieve information from the database to display in your Flask application
* Allowing user input to write to the database - enable users to submit data through your Flask application, which gets stored in the database
* Populating the database from a CSV file (text file format that uses commas to separate values which stores tabular data)
* Web Forms for Database Interaction

* Allow users to input data through a user-friendly interface. Use form libraries like WTForms to create web forms in your Flask application which define the structure of the data you want to collect, and they can include various field types (text, checkboxes, dropdowns, etc.).

    
## Connecting a Database to a Flask App

* Understand a basic script for testing database connection
* Verify the connection by running the script and checking the browser

## SQL and Flask Objects

# Database and SQLAlchemy:
- Use SQLite for data management.
- SQLAlchemy facilitates interaction with the database.
- Debugging aids in object examination.

# Imports and Flask Objects:
- Flask app object: The Flask app object represents the core of a Flask web application. It is created using the Flask() constructor and serves as the central point for configuring the application, defining routes, and managing various components.

- SQLAlchemy (SQLA) db object: The SQLAlchemy db object is an instance of the SQLAlchemy class, acting as a bridge between a Flask application and the database. It simplifies database interactions by allowing developers to work with Python objects as if they were database entities, providing a high-level interface for tasks like defining models and performing queries.

# Model Definition - User Class:
Inherit from db.Model.
Define columns, initialization, and CRUD methods.
Discuss @property, @<column>.setter, and purpose of CRUD methods.

# Initial Data:
Use db.create_all() for table initialization.
Comment on the process.

# User Object Constructors:
Implement try/except for exception handling.
Discuss the purpose of constructors.

# Checking for Credentials and ORM Queries:
Verify credentials using ORM Query objects.
Comment on the purpose of ORM queries.

# Creating a New User:
Use SQLAlchemy and custom methods for user creation.
Comment on the process.

# Reading Users Table:
Employ SQLAlchemy's query.all for data retrieval.
Comment on the purpose and List Comprehension.

# Introduction to Python and Flask
- Python: A versatile programming language known for its readability and wide range of applications. Essential in web development, data science, automation, and more.
- Flask:  A lightweight web framework for Python, ideal for small to medium web applications. Flask allows for easy setup and flexible application structure.

## Setting up the Environment
- Installing Python: Download Python from python.org. Choose the version compatible with your operating system.
- Installing Flask: Use pip, Python’s package installer. Run pip install flask in your command line.

## Introduction to SQL and Databases
- SQL Basics: A language used to communicate with databases. Commands like SELECT, INSERT, UPDATE, and DELETE allow for data manipulation.
- Database Tables and Relationships: Tables store data in rows and columns. Primary keys uniquely identify a row, while foreign keys establish a link between tables.

## Creating a User Table in SQL




```python
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    password VARCHAR(50) NOT NULL,
    email VARCHAR(50)
);
```

- Security best Practices: Store passwords securely using hashing (e.g., bcrypt). Never store plain text passwords.

## Integrating Flask with SQL
- Flask-SQL_Alchemy: An extension that adds support for SQLAlchemy to your Flask application.
- Connecting Flask to a Database: python


```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(name)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///site.db'
db = SQLAlchemy(app)
```

## CRUD Operions in Flask
- Create (Inserting Data): Python


```python
@app.route('/add_user', methods=['POST'])
def add_user():
    username = request.form['username']
    password = request.form['password']
    new_user = User(username=username, password=password)
    db.session.add(new_user)
    db.session.commit()
    return 'User added'
```

- Read (Querying Data): Python


```python
@app.route('/users')
def users():
    users = User.query.all()
    return render_template('users.html', users=users)
Update and Delete: Similar structure to Create and Read, with appropriate SQL commands.
Building a User Registration Form
HTML Form:
html
Copy code
<form action="/add_user" method="post">
    <input type="text" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <input type="submit" value="Register">
</form>
```

- Handling Form Data in Flask: Use the request object to access form data

## Conclusion
Recap the importance of integrating Flask with SQL for user management. Encourgae further exploration of Flask and more comples SQL operations.

## Database and SQLAlchemy

### Overview
This section introduces the use of SQLAlchemy with a SQLite database to demonstrate how programs interact with data, emphasizing the iterative, interactive nature of processing information and managing data using classes and tables. It aligns with the College Board's emphasis on program usage, managing data, insight, filter systems, and application in programming.

## Imports and Flask Objects
### Purpose
The imports initialize the Flask app and SQLAlchemy, which are essential for creating a web application connected to a database.
### Flask app object
This is the core of the Flask application, used for handling web requests.
### SQLAlchemy db object
This is the core of the ORM, used for database interactions in an object-oriented manner.

## Model Definition

### Class User and db.Model Inheritance
This is where the User model is defined, inheriting from db.Model of SQLAlchemy, which facilitates database operations in an object-oriented way.
### Init Method and Property Decorators
The init method sets up the initial state of the User object, while @property and setter methods define how to access and modify the object's attributes.
### CRUD Methods
Methods for creating, reading, updating, and deleting records in the database are defined here, making use of SQLAlchemy's ORM capabilities for streamlined database interactions.

## Inital Data

### Function initUsers
This function initializes the database and populates it with test data using User model instances. The use of try/except blocks ensures graceful handling of errors, such as duplicate data.
Check for Given Credentials

### Fuctionality
The find_by_uid function retrieves a user from the database based on the unique identifier, while check_credentials validates user credentials.

### SQLAlchemy Use
Utilizes SQLAlchemy's query-building methods for efficient database searches.

## Create a New User

### Process
The create function demonstrates adding a new user to the database, showcasing input handling, object initialization, and error management.

### User Creation Logic
Involves checking for existing users, handling date inputs, and using the custom create method of the User model for database insertion.

## Reading Users Table

### Method
The read function uses SQLAlchemy's query.all method to retrieve all users from the database.

### Data Formatting
Implements list comprehension to format the user data into a JSON-friendly structure, making it easier to handle in web applications.

## Hacks (Suggestions for Extension)
### Persionalization
Encourages reader to implement these examples in their own projects, adapting and expanding upon the CRUD fuctionality.
### Enhancements
Suggests adding update and delete functionalities to provide a comprehensive undetstanding of CRUD operations in a databsae context
