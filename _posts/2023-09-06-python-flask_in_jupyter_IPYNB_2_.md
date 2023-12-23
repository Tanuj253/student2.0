---
toc: True
comments: False
layout: post
title: Python/Flask in Jupyter
description: Quick launch into Flask, Functions, List, API, and HTML.
courses: {'csp': {'week': 5, 'categories': ['1.A', '3.B']}}
categories: ['C4.0']
type: devops
---

## Introduction

Welcome to this journey into the world of web servers and the Flask framework! In the previous weeks, you've successfully set up a web server using GitHub Pages, converting Jupyter Notebooks into Markdown for a seamless online presentation. Today, we'll take that knowledge to the next level as we dive into creating your very own web server using Flask.

### Understanding Web Servers
What is a Web Server?

Traditionally, we had librarians at libraries that would help you find books or information. Today in the digital world, thousands upon thousands of home pages, search engines, and digital archives have been built using web servers.

### GitHub Pages vs. Flask

You've already experienced a form of web server through GitHub Pages. Think of GitHub Pages as a library that has established rules for publishing Markdown notes and Jupyter Notebooks neatly on a bookshelf.

Now, let's introduce Flask, your personal web server. Flask can create and manage any type of content, including customizing everything according to your preferences, and even serve additional information (like a database with APIs).

The Flask Framework
Flask is a micro web framework written in Python. It's designed to be minimal and easy to use, making it perfect for building web applications, APIs, and, yes, even your web server. Today, we will start with the basics of Flask and see how it empowers you to create and manage web content.

##  Our Goals for Today
Here's what we'll accomplish in this session:

- Create a minimal Flask server.
- Explore the Python/Flask process.
- Access data from our Flask server using Python.
- Access data from our Flask server using JavaScript.
- Learn how to stop the Python/Flask process gracefully.

Note: Jupyter magic commmand `%%python --bg` that follows runs the server in background.  This enables us to continue interacting with the subsequent Notebook cells.


```python
%%python --bg

from flask import Flask, jsonify
from flask_cors import CORS

# initialize a flask application (app)
app = Flask(__name__)
CORS(app, supports_credentials=True, origins='*')  # Allow all origins (*)

# ... your existing Flask

# add an api endpoint to flask app
@app.route('/api/data')
def get_data():
    # start a list, to be used like a information database
    InfoDb = []

    # add a row to list, an Info record
    InfoDb.append({
        "FirstName": "John",
        "LastName": "Mortensen",
        "DOB": "October 21",
        "Residence": "San Diego",
        "Email": "jmortensen@powayusd.com",
        "Owns_Cars": ["2015-Fusion", "2011-Ranger", "2003-Excursion", "1997-F350", "1969-Cadillac"]
    })

    # add a row to list, an Info record
    InfoDb.append({
        "FirstName": "Shane",
        "LastName": "Lopez",
        "DOB": "February 27",
        "Residence": "San Diego",
        "Email": "slopez@powayusd.com",
        "Owns_Cars": ["2021-Insight"]
    })
    
    return jsonify(InfoDb)

# add an HTML endpoint to flask app
@app.route('/')
def say_hello():
    html_content = """
    <html>
    <head>
        <title>Hellox</title>
    </head>
    <body>
        <h2>Hello, World!</h2>
    </body>
    </html>
    """
    return html_content

if __name__ == '__main__':
    # starts flask server on default port, http://127.0.0.1:5001
    app.run(port=5001)

```

### Show Python/Flask process
This script discovers the running flask process


```python
%%script bash

# After app.run(), see the the Python process
lsof -i :5001
# see the the Python app
lsof -i :5001 | awk '/Python/ {print $2}' | xargs ps


```

### Access API with Python
This script extracts data from Web Server.


```python
import requests
res = requests.get('http://127.0.0.1:5001/api/data')
res.json()
```

### Access API with JavaScript
This code extracts data "live" from a local Web Server with JavaScript fetch.  Additionally, it formats the data into a table.

<!-- Head contains information to Support the Document -->


<!-- HTML table fragment for page -->
<table id="demo" class="table">
  <thead>
      <tr>
          <th>First Name</th>
          <th>Last Name</th>
          <th>Residence</th>
      </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>

<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");
  
  // prepare URL
  url = "http://127.0.0.1:5001/api/data";

  // set options for cross origin header request
  const options = {
    method: 'GET', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, *cors, same-origin
    cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'include', // include, *same-origin, omit
    headers: {
      'Content-Type': 'application/json',
    },
  };

  // fetch the API
  fetch(url, options)
    // response is a RESTful "promise" on any successful fetch
    .then(response => {
      // check for response errors and display
      if (response.status !== 200) {
          console.error(response.status);
          return;
      }
      // valid response will contain json data
      response.json().then(data => {
          console.log(data);
          for (const row of data) {
            // tr and td build out for each row
            const tr = document.createElement("tr");
            const firstname = document.createElement("td");
            const lastname = document.createElement("td");
            const residence = document.createElement("td");
            // data is specific to the API
            firstname.innerHTML = row.FirstName; 
            lastname.innerHTML = row.LastName; 
            residence.innerHTML = row.Residence; 
            // this builds each td into tr
            tr.appendChild(firstname);
            tr.appendChild(lastname);
            tr.appendChild(residence);
            // add HTML to container
            resultContainer.appendChild(tr);
          }
      })
  })
  
</script>


### Kill Python/Flask process
This script ends Python/Flask process


```python
%%script bash

lsof -i :5001 | awk '/Python/ {print $2}' | xargs kill -9

```

## Hacks
Edit, stop and start the web server.
- Add to the Home Page
- Add your own information to the Web API
- Use from Template to start your own Team Flask project https://github.com/nighthawkcoders/flask_portfolio
