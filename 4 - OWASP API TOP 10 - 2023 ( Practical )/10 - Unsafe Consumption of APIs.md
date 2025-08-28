---
created: 2025-08-28T13:35:00
---
---


## Summary 
---
* Developers often trust third-party APIs without verifying their security, leading to potential exploits and impact on their systems
  
* Common Security weakness include weak transport security, authentication/authorization and input validation and sanitization 
  
* Potential impacts includes exposure of sensitive information and various types of injections 

* Example attack : an `SQL Injection` via **third-party services**, unintentionally leaking sensitive data due to blind redirects , and input injection through unsafe repository names 


## Code
---
```
import requests
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/api/quote', methods=['GET'])
def get_quote():
    # Fetch a quote from a third-party API
    response = requests.get('https://api.quotable.io/random')

    # Check if the response is successful and parse the JSON data
    if response.status_code == 200:
        data = response.json()
        quote = data.get('content', 'No quote found')

        # Return the quote without proper validation and sanitization
        return jsonify({'quote': quote}), 200
    else:
        return jsonify({'error': 'Unable to fetch a quote'}), 500

if __name__ == '__main__':
        app.run(host='0.0.0.0', port=9882)
```

* #### Above code
	* we are not checking the incoming requests and their validations
	* a basic implemented logic that should not be in production 
	* no proper validations and preventions for attacks such as injections, BOPLA, etc 


