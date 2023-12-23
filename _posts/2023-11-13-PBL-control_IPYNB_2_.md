---
layout: post
toc: True
title: API/Control
description: Between Model and View is Control.  In this Web application it will be handled by APIs.  Data will be input and displayed through View and stored in Model.  Control will broker information between projects.
permalink: /cpt/api
---

## API/Control Terms
> APIs work with methods to GET, POST, PUT, and UPDATE data.  Control helps with Requests, Response, and handling JSON.  Control is glue layer, thus the term Model-View-Control (or MVC).
- POST APIs interact with CREATE methods in Model.
- GET with READ
- PUT with UPDATE.
- DELETE with DELETE.

> During development it is best to work with Model and Control without involving View initially.   To support this type of development ...
- Become familiar with [Postman](https://www.postman.com/downloads/)
- FYI, as an alternative you can become familiar with working with APIs through ***curl***

### Resource
[API Docs](https://flask-restful.readthedocs.io/en/latest/api.html)

### Control/API code
> ***Control/API concepts*** are receiving and API request and returning a response.  

1. [Define API flask objects (api/user.py)](https://github.com/nighthawkcoders/flask_portfolio/blob/main/api/user.py#L7-L11).  Flask contains object that help in API definition.
2. [Register API objects (main.py)](https://github.com/nighthawkcoders/flask_portfolio/blob/main/main.py#L11-L23).  Every Flask object in the project needs to be registered with the "main" objects.
3. [Create/POST method](https://github.com/nighthawkcoders/flask_portfolio/blob/main/api/user.py#L14-L54).  Post method contains a lot of checking code, but ultimately it creates a database row in the Model and returns that row to the View.
4. [Read/GET method](https://github.com/nighthawkcoders/flask_portfolio/blob/main/api/user.py#L14-L54).  This shows off Object Relational Manager performing a User (class operation) to extract all records from the table and putting them into User objects.
5. [Define API endpoints](https://github.com/nighthawkcoders/flask_portfolio/blob/main/api/user.py#L62-L64).  Endpoints are somewhat patterns to be matched.  Note, url_prefix at top of file supplies "/api/users" prefix for each pattern.  Each pattern when matched invokes the correspond class.  Methods are defined in class to correspond to expectations (POST, GET, UPDATE, DELETE).



```python
from flask import Blueprint, request, jsonify
from flask_restful import Api, Resource # used for REST API building
from datetime import datetime

from model.users import User

# blueprint, which is registered to app in main.py
user_api = Blueprint('user_api', __name__,
                   url_prefix='/api/users')

# API docs https://flask-restful.readthedocs.io/en/latest/api.html#id1
api = Api(user_api)
class UserAPI:        
    class _Create(Resource):
        def post(self):
            ''' Read data for json body '''
            body = request.get_json()
            
            ''' Avoid garbage in, error checking '''
            # validate name
            name = body.get('name')
            if name is None or len(name) < 2:
                return {'message': f'Name is missing, or is less than 2 characters'}, 210
            # validate uid
            uid = body.get('uid')
            if uid is None or len(uid) < 2:
                return {'message': f'User ID is missing, or is less than 2 characters'}, 210
            # look for password and dob
            password = body.get('password')
            dob = body.get('dob')

            ''' #1: Key code block, setup USER OBJECT '''
            uo = User(name=name, 
                      uid=uid)
            
            ''' Additional garbage error checking '''
            # set password if provided
            if password is not None:
                uo.set_password(password)
            # convert to date type
            if dob is not None:
                try:
                    uo.dob = datetime.strptime(dob, '%m-%d-%Y').date()
                except:
                    return {'message': f'Date of birth format error {dob}, must be mm-dd-yyyy'}, 210
            
            ''' #2: Key Code block to add user to database '''
            # create user in database
            user = uo.create()
            # success returns json of user
            if user:
                return jsonify(user.read())
            # failure returns error
            return {'message': f'Processed {name}, either a format error or User ID {uid} is duplicate'}, 210

    class _Read(Resource):
        def get(self):
            users = User.query.all()    # read/extract all users from database
            json_ready = [user.read() for user in users]  # prepare output in json
            return jsonify(json_ready)  # jsonify creates Flask response object, more specific to APIs than json.dumps

    # building RESTapi endpoint
    api.add_resource(_Create, '/create')
    api.add_resource(_Read, '/')
```

### Testing APIs
> ***Backend Testing*** of APIs is best done through Browser for simple GET APIs, but other API methods (POST, UPDATE, DELETE) will require a tool like PostMan.

1. [Download Postman](https://www.postman.com/downloads/).  This tool test APIs effectively on localhost and is great aid for debugging.

2. [Main.py runtime configuration](https://github.com/nighthawkcoders/flask_portfolio/blob/main/main.py#L44-L47).  This configuration is setup to produce same port and localhost as deployment.

***`Run` locally as you develop*** Select main.py file in VSCode and press Play button, or press down arrow next to Play button to activate Debug testing.  The below dialog will appear in Terminal, though IP address will match you machines.

```bash
(base) machine:flask_portfolio user$  cd /Users/user/vscode/flask_portfolio ; /usr/bin/env /Users/user/opt/anaconda3/bin/python /Users/user/.vscode/extensions/ms-python.python-2022.20.2/pythonFiles/lib/python/debugpy/adapter/../../debugpy/launcher 61127 -- /Users/user/vscode/flask_portfolio/main.py 
 * Serving Flask app "__init__" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on all addresses.
   WARNING: This is a development server. Do not use it in a production deployment.
 * Running on http://192.168.1.75:8086/ (Press CTRL+C to quit)
 * Restarting with watchdog (fsevents)
 * Debugger is active!
 * Debugger PIN: 403-552-045
```

***`Test` API GET locally with Postman***.  Observe that tests may be saved.  
![](images/postmain_read_get_users.png)

***`Test` API POST locally with Postman***.  In this case, Postman can be used to add new records to the table.  Observe options to pass data using Body raw-json.
![](images/postman_create_post_users.png)

## Hacks
Objective of these hacks is to complete full stack.  This include Model (flask database), View (github pages markdown), and Control (flask API endpoint).

1. Make Create and Read API endpoints for your project
2. Test API endpoints by creating test cases in Postman
3. Build a Frontend for Create and Read endpoints
4. Make a 30-60 second video showing this work
