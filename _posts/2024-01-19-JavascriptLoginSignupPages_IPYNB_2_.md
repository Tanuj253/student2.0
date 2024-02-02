---
toc: True
comments: True
layout: post
title: Javascript Login and Signup Pages Team Teach
description: P2 Group, Alisha, Peyton, Anika, Sharon, Tara
courses: {'compsci': {'week': 20}}
type: hacks
---

Errors: 

- 200 (OK): Returned on successful login or signup.
- 401 (Unauthorized): Returned when a user enters incorrect login credentials.
- 403 (Forbidden): Returned when a user tries to access a resource they don't have permission for.
- 409 (Conflict): Can be used if a new user is trying to register with an email/username that already exists.

- Error Handling: Implement proper error handling in Flask to return meaningful error messages and appropriate HTTP status codes.


```python

.authorizeHttpRequests(auth -> auth
	.requestMatchers("/authenticate").permitAll()
    .requestMatchers("/mvc/person/update/**", "/mvc/person/delete/**").authenticated()

    // .requestMatchers("/api/person/post/**", "/api/person/delete/**").authenticated()
    // Removed so anyone without a cookie can post

    .requestMatchers("/api/person/delete/**").authenticated()
    .requestMatchers("/**").permitAll()
)
```

Secruity: 

- this code sets up security policies for a web application, dictating which endpoints are accessible to all users and which are restricted to only those who are authenticated.
- 'auth' represents the authorization configuration(rules/polices that say what users are allowed to do/access).
- it's important to have since it can keep important details like user data protected, while more public parts of the application remain accessible to everyone. 


```python
@PostMapping( "/post")
// @RequestParam is why user input needs to be a parameter
public ResponseEntity<Object> postPerson(@RequestParam("email") String email,
                                         @RequestParam("password") String password,
                                         @RequestParam("name") String name,
                                         @RequestParam("dob") String dobString) {
    Date dob;
    // dob handling
    try {
        dob = new SimpleDateFormat("MM-dd-yyyy").parse(dobString);
    } catch (Exception e) {
        return new ResponseEntity<>(dobString +" error; try MM-dd-yyyy", HttpStatus.BAD_REQUEST);
    }
    // A person object WITHOUT ID will create a new record with default roles as student
    Person person = new Person(email, password, name, dob);
    personDetailsService.save(person);
    return new ResponseEntity<>(email +" is created successfully", HttpStatus.CREATED);
}
```

Endpoint Mapping 
- this code is an endpoint for creating new person records in the application 
- processes data submitted by users (such as email, password, name, and date of birth)
- the 'new person' represents the new account/user being made\n",
- this is important for the users to have their own personal data and profile, and the secruity we previously mentioned is important to protect this data from people who dont need access to it. "

SASS Styling 


```python
form {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    background: #4e7dc3;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);

    p {
        margin-bottom: 15px;

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        input {
            width: 100%;
            padding: 8px;
            box-sizing: border-box;
            margin-bottom: 8px;
            border: 1px solid #8d8d8d;
            border-radius: 4px;
        }

        button {
            background: #2e406b;
            color: #fff;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
    }
}
```

In custom-styles.scss...

<img width="344" src="https://github.com/sharonkodali/CPTproject/assets/39902320/f2fb216d-63d6-453f-b432-02961a7b5109">


before SASS styling...

<img width="344" src= "https://github.com/sharonkodali/CPTproject/assets/39902320/2a6cd738-f25a-4945-bdb1-cfce9195ee91">

## Required HTML properties
- Most HTML Elements:
    - Class: used to assign html elements to there own SASS
    - ID: used mostly in javascript to identify a single element in an entire page full of HTML
- Input Fields:
    - Placeholder: puts text inside an input to help the user what to input
- Buttons:
    - onclick: runs a javascript function when clicked
    - note: onclick can be replaced with event listeners, although onclick is simpler to code
## Essential HTML Elements Required
- Container: The primary Div that holds all the elements inside of it, first layer
- Card: A Div that will hold username, password buttons
- Input fields and buttons: Where the user will put there information/login
- Common SASS that a Container will have:
    - display
    - justify-content/align items
    - color
# Simplified Code
```html
<div class="CONTAINER">
    <div class="CARD">
        <h3>Login</h3>
        <input id="signInEmailInput" class="input" placeholder="Email">
        <input id="signInPasswordInput" class="input" placeholder="Password">
        <button class="signInButton" onclick="login_user()">Login</button>
    </div>
</div>
```
# Basic HTML Login
<div class="CONTAINER">
    <div class="CARD">
        <h3>Login</h3>
        <input id="signInEmailInput" class="input" placeholder="Email">
        <input id="signInPasswordInput" class="input" placeholder="Password">
        <button class="signInButton" onclick="login_user()">Login</button>
    </div> 
</div>
# userDbRequest Function (What is its Purpose?)
- Adds data to our frontend html fragment, which will store our data.
- Creating data in our frontend database view which users can see with authentication (login).
html fragment will be our table which we can input with data using our function.
<!-- HTML table fragment for page -->
<table>
  <thead>
  <tr>
    <th>Name</th>
    <th>ID</th>
    <th>Age</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>


```python
type="module">
    // uri variable and options object are obtained from config.js
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    function login_user(){
        // Set Authenticate endpoint
        const url = uri + '/api/users/authenticate';
        // Set body of request to include login data from DOM
        const body = {
            uid: document.getElementById("uid").value,
            password: document.getElementById("password").value,
        };
        // Change options according to Authentication requirements
        const authOptions = {
            ...options, // This will copy all properties from options
            method: 'POST', // Override the method property
            cache: 'no-cache', // Set the cache property
            body: JSON.stringify(body)
        };
        // Fetch JWT
        fetch(url, authOptions)
        .then(response => {
            // handle error response from Web API
            if (!response.ok) {
                if (response.status === 401) {
                    // Unauthorized - Redirect to 401 error page
                    window.location.href = "{{site.baseurl}}/errors/401.html";
                } else if (response.status === 403) {
                    // Forbidden - Redirect to 403 error page
                    window.location.href = "{{site.baseurl}}/errors/403.html";
                } else if (response.status === 404) {
                    // Not Found - Redirect to 404 error page
                    window.location.href = "{{site.baseurl}}/error/404.html";
                } else {
                    // Handle other error responses
                    const errorMsg = 'Login error: ' + response.status;
                    console.log(errorMsg);
                }
                return;
            }
            // Success!!!
            // Redirect to the database page
            window.location.href = "{{site.baseurl}}/data/database";
        })
        // catch fetch errors (ie ACCESS to server blocked)
        .catch(err => {
            console.error(err);
        });
    }
    // Attach login_user to the window object, allowing access to form action
    window.login_user = login_user;

```

Explanantion
the url vairable is set to a specific enpointendpoitn for user authentication
The body retrieves the user id and password from the HTML
creates a data object with the user id and the password
uses the fetch function to make a POST request to the aunthetication endpoint('url') with the specified autho0options
if the response is succesful, it redirects the user to the database page.
Checks if response status is 401, 404, or 403 and redirects to corrsponding page with the endpoint
