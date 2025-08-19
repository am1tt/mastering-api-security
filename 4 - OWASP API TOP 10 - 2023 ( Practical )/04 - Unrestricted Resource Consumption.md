---
created: 2025-08-19 T12:20
---
----



## Summary 
---
>basically API'S that do not restrict and add limitations on resource causing leakages and resource loss

* Commonly found API'S that do not *limit* client interactions or *resource consumption* , often going **unnoticed**

* if exploited can lead to **Denial of Service** (Dos) due to starvation or impact service providers billing 

>  An API is vulnerable if limits for resource , such as execution timeouts or maximum or allocable memory (it can still be misused and intercepted )

* Example attacks such as , including available memory , through large file uploads , brute-forcing credit card activation 


## Example Attacks
---
- large file uploads 
- brute-forcing credit card activation 
- infinite requests 
- cloud billing abuse 



## Vulnerable Code 
---
```
from flask import Flask, request, jsonify
from flasgger import Swagger, swag_from

app = Flask(__name__)
swagger = Swagger(app)

@app.route('/api/v1/images', methods=['POST'])
@swag_from({
    'responses': {
        201: {
            'description': 'Image uploaded and thumbnails created.'
        }
    },
    'parameters': [
        {
            'name': 'image_data',
            'in': 'formData',
            'type': 'string',
            'format': 'base64',
            'required': 'true',
            'description': 'Base64 encoded image data'
        }
    ],
    'consumes': [
        'multipart/form-data'
    ],
    'tags': ['Images']
})
def upload_image():
    image_data = request.form.get('image_data')
    if not image_data:
        return jsonify({"error": "Missing image data"}), 400

    # Process the image and create thumbnails
    # ...

    return jsonify({"message": "Image uploaded and thumbnails created."}), 201


if __name__ == '__main__':
    app.run(host='0.0.0.0', port=9876)
```

* ### Above code 
	* there is not rate limiting implemented and restrictions 
	* it is vulnerable to **Denial of Service ( Dos )** 
	* Multiple images can be uploaded , resulting in resource exhaustions and client side and server side functional issues 


