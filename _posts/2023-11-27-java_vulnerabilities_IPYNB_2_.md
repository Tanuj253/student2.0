---
layout: post
title: Java, Exploits | CORS | DotEnv
description: Web exploits and how to protect against them
courses: {'csa': {'week': 15}}
categories: ['C2.2']
type: devops
author: William Wu
---

## Ideas
Using two or more systems in computing requires addition security.

- CORS
  - MVC Config
  - Nginx Config

Using external APIs requires managing access tokens on server, without putting these tokens into GitHub.

- DotEnv
  - Managing Secret Keys
  - Storing key on localhost and AWS server

Below is article that talks about attacks.

## Overview

Making a website is fun and all but if you aren't careful, it may be vulnerable to exploits.
Why should you care? Because many of your user's important info like name, email, and dob may be leaked. Accidently leaking info can make people to stray away from using your website. Let's try to not let that happen.

## Basic Knowledge

HTTP: a communication protocol to deliver data over the internet
- The client makes a request (ex: POST, GET), server consults database and returns an output

Database: collection of information that is organized, where the server can quickly query data and add/delete data
- SQL is the language used for managing these databases (which are made up of tables each with data)
- Databases store data in the website, such as passwords or FLAGS
- SQL syntax: 
    - SELECT * FROM table_name
- Selects all data from specific table (this can be exploited)

Proxy: a server that redirects your browsing activity before accessing the web
- The web request first gets sent to the proxy and then to the web instead of straight to the web, masking IP address
- Also provides a way to edit the request before it leaves (BURP SUITE)


## Types of Basic Web Insecurities

### Being an idiot and having sensitive info on html or js
- Hardcoded Credentials: Storing usernames, passwords, API keys, or other sensitive credentials directly in the source code

- Client-Side Storage: Storing sensitive data in client-side storage mechanisms like cookies, local storage, or session storage without proper encryption or security controls is risky. Attackers can manipulate or extract this data using techniques like cross-site scripting (XSS) attacks.

- Sensitive info in JS: Including sensitive information within inline JavaScript code can expose it to attackers. Inline scripts can be easily viewed or modified by anyone with access to the page source.

## Simple fixes (common sense)
- Not having important stuff on HTML (encoding it in a cipher doesn't fix)

- Proper encryption and configuration of JWT cookies

- Not having sensitive info in JS (don't like console.log a cred and stuff duh)

## Big Boy exploits

### SQL Injection
Before you read anything about SQL Injection, make sure you understand everything in the "Basic Knowledge" section

#### Definition
SQL injection is a type of web application vulnerability that occurs when an attacker maliciously inserts or "injects" unauthorized SQL code into a database query. It takes advantage of improper handling of user-supplied input, typically in a web application that interacts with a database using SQL queries.

Confused? Here is an example:

#### Example

Let’s say you want to buy a gift on Amazon and go to this link https://amazon.com/products?category=Gifts

This causes the SQL query to retrieve details from the database

SELECT * FROM products WHERE category = 'Gifts' AND released = 1

Where
- "*" = everything
- Products = table name
- The restricts are that the category is ‘Gifts’ and released is equal to 1 (in this example this means the gift is released)

Now what if we want to get stuff from the database that isn’t released? How would we bypass the “released=1”?

This is where SQL injection comes into play! Before we tackle how to do it, let’s go over some syntax

#### Syntax

- Two dashes (- -) is an SQL comment
- "||" is concatenate
- SELECT * FROM users WHERE username = 'william' AND password = 'Password1!'
    - Stuff between single quotes are strings
    - Everything else is an SQL query

IMPORTANT
- A single quote (‘) helps you break out of a string and write more SQL query!
- “AND” statement always runs before “OR” statement 

Now we are getting into the good stuff

#### How the attack works

Remember that a login page uses a sql query to search the db if the user and password entered is correct. Remember this syntax: SELECT * FROM users WHERE username = 'william' AND password = 'Password1!'
Whatever you enter into the login page will be set as "username" and "password". My syntax examples has username as 'william' and password as 'Password1!'. Notice how they are between single quotes. Anything between quotes is a string and everything outside of it is a SQL query. 

But what if you enter a single quote ' into the login page? Then you will break out of the string and be able to run SQL queries! This is the basis of the two payloads below:

- OR Payload: Having an OR statement that is always true

Let's say we enter admin' OR '1'='1' for the username field. Then our sql query will look like this: SELECT * FROM users WHERE username = 'admin'' OR '1'='1' AND password = 'Password1!'

Now remember how a single quote ' will break out of a string and run sql query commands. We are basically running OR '1'='1' which will obviously always evaulate as true. And since a OR statement will be true if one side of the argument is true, we basically bypass both the username and the password. Thus both fields could be incorrect but we would still be able to login!

- Comment Payload: Commenting parts of the query in order to bypass stuff

Remember how two dashes (--) is an SQL comment? Well we can abuse this to bypass some info. 

For example, we can bypass the password by inputting admin'-- in the username box. The sql query would be: SELECT * FROM users WHERE username = 'admin''-- AND password = 'Password1!'

This will lead to the AND password section not being executed. Thus we don't need to input the correct password in order to login. In fact, we don't even need to input a username as sql validates it as an empty string and the whole expression would take the first user. So uh you won't need to have either info.

#### How to protect

Learning how the exploits are cool and all but the whole point of this blog is to learn how to protect your website. Here is one way you can protect against sql injections

Use of Prepared Statements (with Parameterized Queries)
- Instead of directly embedding user-supplied data in SQL queries, prepared statements use placeholders for the input data. The SQL statement and the input data are sent separately to the database, preventing malicious input from altering the structure of the query.

Here's an example of not protected code:


```java
import java.sql.*;

public class DatabaseExample {

    // Assuming you have established a database connection
    
    // Without any protection measures
    public static void createUser(String username, String password) {
        String sql = "INSERT INTO users (username, password) VALUES ('" + username + "', '" + password + "')";
        
        try (Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/db_name", "username", "password");
             Statement statement = connection.createStatement()) {
            
            statement.executeUpdate(sql);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        // Example usage
        String username = "john";
        String password = "secretpassword'; DROP TABLE users; --";
        
        createUser(username, password);
    }
}
```

With protection


```java
import java.sql.*;

public class DatabaseExample {

    // Assuming you have established a database connection
    
    // Option 1: Use of Prepared Statements (with Parameterized Queries)
    public static void createUser(String username, String password) {
        String sql = "INSERT INTO users (username, password) VALUES (?, ?)";
        
        try (Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/db_name", "username", "password");
             PreparedStatement statement = connection.prepareStatement(sql)) {
            
            statement.setString(1, username);
            statement.setString(2, password);
            
            statement.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        // Example usage
        String username = "john";
        String password = "secretpassword";
        
        createUser(username, password);
    }
}
```

In this example, we have a createUser method that takes username and password as parameters. We define a SQL query string with placeholders (?) for the values.

Inside the method, we establish a connection to the database using JDBC. We then create a prepared statement using the SQL query string. The setString method is used to set the values for the placeholders.

Finally, we execute the update using executeUpdate to insert the user into the database. Any potential SQL injection attempts are prevented because the parameterized queries ensure that the user input is treated as data and not as part of the SQL syntax.

There are a lot more ways to protect against sql injection. Feel free to research them:

- Use of Properly Constructed Stored Procedures

- Allow-list Input Validation

- Escaping All User Supplied Input

## XSS (Cross Site Scripting)

The use of sending JavaScript that can be executed in the browser.
Can modify the page, send more HTTP requests, and access cookies.

Basically some websites are dumb and think < script> is still html

Why should we care? Well JS is powerful. It can modify the html through the DOM, note down user inputs, and even read website cookies. It has the power and potential to do basiclaly anything, making it very crucial to protect against it. Here is a real life example of the exploit in action: https://www.youtube.com/watch?v=2GtbY1XWGlQ&t=0s

### Types

- Reflected XSS: user input is identified as html but it's now and it's reflected back. For example if you input "< script>alert("bruh")< /script> into a text box and send a GET request, the alert JS will be run.
- Stored XSS: same as reflected xss but instead of being reflected back to the user, it's stored in the db. The user it then affected by the modified db. This is even more dangerous than reflected xss because it injects everyone who views the injected page! 

### How to fix

You might think it's a simple removing the ability to input script tags but it's not because event handlers exist. Filtering '<' and '>' techincally works but could cause other problems. 

So how do you actually fix? You could simply use a library called DOMPurify that filters everything for you :D

[How to use DOMPurify](https://github.com/cure53/DOMPurify/blob/main/README.md)

## Note
Most of my knowledge from this blog came from my experience completing web exploit ctf (capture the flag) problems. This is just skimming the surface of web vulnerabilities but I hope it makes you more mindful of your website's security. Also please don't use the knowledge I gave you to go hack other websites lmao. This is for educational purposes ONLY. Go compete in CTFs (or do the hacks) if you really wanna exploit a website :D. 

### Resources
- [SQL Injection](https://portswigger.net/web-security/sql-injection#:~:text=SQL%20injection%20(SQLi)%20is%20a,not%20normally%20able%20to%20retrieve)
- [Really good youtube SQL Injection vid](https://www.youtube.com/watch?v=2OPVViV-GQk)
- [Prevent SQL Injection](https://security.berkeley.edu/education-awareness/how-protect-against-sql-injection-attacks)
- [XSS Website](https://portswigger.net/web-security/cross-site-scripting)
- [XSS Vid](https://www.youtube.com/watch?v=EoaDgUgS6QA)
- Google :drooling_face:
- Chatgpt for definitions and code examples <3

## Hacks

- SQL Inject this site: https://demo.testfire.net/login.jsp and blog your solution.
- Play this fun game Google made on XSS: https://xss-game.appspot.com/. Beat all six levels and blog about them.

- Extra Credit: Create a PicoCTF account: https://play.picoctf.org/practice?category=1&page=1 -> go to Practice -> go to Web Exploit -> Complete any 7 challenges and blog about them. Many of these topics won't be covered in this blog but it will show you common web exploitations. Feel free to google the answers whenever you get stuck but please attempt them first.



