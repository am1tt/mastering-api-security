---
created: 2025-08-18T12:35:00
---
---



##  🧠Overview 
---
 * Refers to **BOLA** , considering both `user based` and `object based`
 * Property refers to *Identifiers* and *objects* basically - `API Endpoints`
 * Authorization refers to API failing to detect the *API endpoint based attacks*
 * allows unauthorized access , sensitive properties , data disclosure
   
 > property , also refers to hidden code written properties , which can be guessable and can be stuffed


## Practical code used
---
```
from flask import Flask, request, jsonify

from flask_httpauth import HTTPBasicAuth

from flasgger import Swagger, swag_from

  

app = Flask(__name__)

auth = HTTPBasicAuth()

Swagger(app)

  

users = {

    "user1": "password1",

    "user2": "password2"

}

  

@auth.get_password

def get_password(username):

    if username in users:

        return users.get(username)

    return None

  

@app.route('/', methods=['GET'])

def home():

    return "user1:password1"

  

@app.route('/api/video/update_video', methods=['PUT'])

@swag_from({

    'responses': {

        200: {

            'description': 'Video updated',

            'schema': {

                'type': 'object',

                'properties': {

                    'id': {'type': 'integer'},

                    'title': {'type': 'string'},

                    'description': {'type': 'string'},

                    'blocked': {'type': 'boolean'}

                }

            }

        }

    },

    'parameters': [

        {

            'name': 'body',

            'in': 'body',

            'required': 'true',

            'schema': {

                'type': 'object',

                'properties': {

                    'description': {'type': 'string'}

                }

            }

        }

    ]

})

def update_video():

    data = request.get_json()

    video = {

        "id": 1,

        "title": "Example Video",

        "description": "A sample video",

        "blocked": True

    }

    allowed_properties = ["description",'blocked']

    for prop in allowed_properties:

        if prop in data:

            video[prop] = data[prop]

    return jsonify(video)

  

if __name__ == '__main__':

    app.run(host='0.0.0.0', port=9875)
```

* in the above code , there are validations implemented , run the code choose the private ip localhost and went to `/apidocs`

* go to the json link and import it in postman , add the ip in the variables and save it and ready to go to *API Test it*


## 🧪 Practical Lab
---
> as this consisted of PUT method , from above code 

* went to the endpoint `/api/video/update_video` and sent a `GET` as expected it was not valid method returned

* sent an `PUT` request and found the objects properties such as blocked and more 
	* this resulted in property disclosure , which normally should not be disclosed

* #### [pre property manipulation](./Practical-Screenshots/03-BOPLA/pre-blocked.png)
	* as in the above *screenshot*
	* the properties are blocked as per the logic , but disclosed 
	* a property like `Blocked` or other object should not be disclosed


* #### [post property manipulation](./Practical-Screenshots/03-BOPLA/post-blocked.png)
	* as property was disclosed , this resulted in manipulation
	* in the above image adding `"blocked":false` and then sending the request resulted in a property change 
		* this will result in bypassing *Authorization* which is a major risk as mentioned earlier



## 💁‍♂️ Conclusion 
---
* Broken object property level authorization , is depending on both BOLA as well as the objects properties mentioned from the logic 
* Disclosing such `properties` will result in `BOPLA`
* *Final note :* the practical images are performed by me , along with that the notes written are non AI and completely written by me 

