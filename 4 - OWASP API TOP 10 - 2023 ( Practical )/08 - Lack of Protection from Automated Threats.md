---
created: 2025-08-25T14:12:00
---
---

## Summary
---
> Automated threats exploits sensitive business flows in API'S and causes harm to business through excessive automated access 

* Traditional protection like rate limiting and captchas become less effective over time , as attackers employ bots-nets and other methods to circumvent them 

* Vulnerable API'S expose business sensitive functionalities without considering the potential harm from excessive automated excess 

* Example attack scenario
	* buying out a limited **stock product** before legitimate users, and exploiting referral programs in ride-sharing apps



## Prevention for automated threats 
---
* to prevent automated threats
	* business should identify flows that could harm the business if used excessively 
	* choose right protection mechanisms such as : 
		* device fingerprinting 
		* human detection 
		* non-human pattern recognition 
		* and blocking IP-addresses of Tor exit nodes and well known proxies 
	* Secure and limit access to APIs consumed directly by machines ,as they are often easy target for the attackers 




## Code 
---
```
from flask import Flask, request, jsonify
from flasgger import Swagger, swag_from

app = Flask(__name__)
swagger = Swagger(app)

@app.route('/api/register', methods=['POST'])
@swag_from({
    'tags': ['User Registration'],
    'parameters': [{
        'name': 'email',
        'description': 'Email address',
        'in': 'json',
        'required': True,
        'type': 'string'
    }, {
        'name': 'password',
        'description': 'Password',
        'in': 'json',
        'required': True,
        'type': 'string'
    }],
    'responses': {
        '200': {
            'description': 'User registered successfully'
        },
        '400': {
            'description': 'Invalid request'
        }
    }
})
def register():
    data = request.json
    if 'email' not in data or 'password' not in data:
        return jsonify({'message': 'Missing email or password'}), 400

    email = data['email']
    password = data['password']

    # Simulate user registration process
    print(f"Registered user: {email} with password: {password}")

    return jsonify({'message': 'User registered successfully'}), 200

if __name__ == '__main__':
        app.run(host='0.0.0.0', port=9880)
```

* ####  in above code , there are issues such as
	* no rate-limiting implemented 
	* no captchas present 
	* no `Middlewares` present 
	* weak **validation**
	* no hashing techniques such as : `base64`, `JWT` implemented
