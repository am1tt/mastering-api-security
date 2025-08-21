---
created: 2025-08-21T11:45:00
---
---


## Summary 
---
> unrestricted access to sensitive business Flows 

* Exposes important business functions without proper restrictions 
* Attackers abuse this flows to gain unfair advantage or attack systems
* Example:  API takes URI as input --> attacker gives a malicious URI --> API fetches sensitive / Internal data 
* Risks : Fraud , privilege escalation , SSRF 
* Fix : Validate inputs , enforce auth , rate-limit , restrict sensitive endpoints 





## Code 
---
```
from flask import Flask, request, jsonify

from flasgger import Swagger, swag_from

import requests

import os

  

app = Flask(__name__)

app.config['SWAGGER'] = {

    'title': 'Image Fetcher API',

    'uiversion': 2

}

swagger = Swagger(app)

  

ALLOWED_SCHEMES = ['http', 'https']

ALLOWED_CONTENT_TYPE = ['image/jpeg', 'image/png', 'image/gif']

MAX_CONTENT_SIZE = 10 * 1024 * 1024  # 10MB

  
  

@app.route('/api/fetch_image', methods=['POST'])

@swag_from({

    'tags': ['Fetch Image'],

    'parameters': [{

        'name': 'url',

        'description': 'URL of the image',

        'in': 'json',

        'required': True,

        'type': 'string'

    }],

    'responses': {

        '200': {

            'description': 'Image fetched and saved successfully'

        },

        '400': {

            'description': 'Invalid request'

        },

        '415': {

            'description': 'Unsupported media type'

        },

        '500': {

            'description': 'Internal server error'

        }

    }

})

def fetch_image():

    data = request.json

    if 'url' not in data:

        return jsonify({'message': 'Missing image URL'}), 400

  

    url = data['url']

  

    try:

        response = requests.get(url, stream=True, timeout=10)

    except requests.exceptions.RequestException as e:

        return jsonify({'message': f'Error fetching image: {e}'}), 500

  

    # Save image

    image_path = os.path.join('images', f'{os.path.basename(url)}')

    with open(image_path, 'wb') as f:

        for chunk in response.iter_content(chunk_size=8192):

            f.write(chunk)

  

    return jsonify({'message': 'Image fetched and saved successfully'}), 200

  
  

if __name__ == '__main__':

        app.run(host='0.0.0.0', port=9878)
```

* the above code does not implemented proper authorization
* can result in SSRF 
* as such logic , can result in SBF 
* API just fetches Whatever the URL attacker gives it 

