---
created: 2025-08-14T14:02:00
---
---



## Code 
---
```
from flask import Flask, jsonify, request

  

app = Flask(__name__)

  

def generate_token(username):

    return 'dsfgfd'

  

@app.route('/login', methods=['POST'])

def login():

    # Get the username and password from the request body

    username = request.json.get('username')

    password = request.json.get('password')

  

    # Validate input

    if not username or not password:

        return jsonify({'message': 'Please provide a username and password.'}), 400

  

    # Authenticate user

    if username == 'admin' and password == 'password':

        token = generate_token(username)

        return jsonify({'token': token}), 200

  

    return jsonify({'message': 'Invalid username or password.'}), 401

  

if __name__ == '__main__':

    app.run(host='0.0.0.0', port=9873)
```


##  🧪Tested 
---
* `/login` endpoint from postman
* rehearsed Broken User Auth concepts such as , **No ratelimit** , **Credentials stuffing** , **missing basic encryptions , JWT , Mitigations** etc


