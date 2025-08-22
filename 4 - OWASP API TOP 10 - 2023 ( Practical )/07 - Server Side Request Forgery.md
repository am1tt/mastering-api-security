---
created: 2025-08-22T12:33:00
---
---


## Security Misconfiguration
---
> attackers look for unpatched flaws , common endpoints , or unprotected files and directories to gain unauthorized access or knowledge of the system

* Security misconfiguration can happen at any level of the API stack and can be detected and exploited using automated tools 

* Security misconfiguration expose sensitive user data and system details , potentially leading to full server compromise 

* Vulnerability can arise from missing security hardening , outdated systems , enabled unnecessary feature , discrepancies in request processing, missing TLS , improper cache control directives and missing or improperly set CORS policies



## Vulnerable Code
---
```
from flask import Flask, request, jsonify, make_response
from flasgger import Swagger, swag_from
import requests
import os

app = Flask(__name__)
app.config['SWAGGER'] = {
    'title': 'Secure Image Fetcher API',
    'uiversion': 2
}
swagger = Swagger(app)

ALLOWED_SCHEMES = ['http', 'https']
ALLOWED_CONTENT_TYPE = ['image/jpeg', 'image/png', 'image/gif']
MAX_CONTENT_SIZE = 10 * 1024 * 1024  # 10MB

@app.after_request
def apply_caching(response):
    return response

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
    if not url.startswith(tuple(ALLOWED_SCHEMES)):
        return jsonify({'message': 'Invalid URL scheme'}), 400

    try:
        response = requests.get(url, stream=True, timeout=10)
    except requests.exceptions.RequestException as e:
        return jsonify({'message': f'Error fetching image: {e}'}), 500

    content_type = response.headers.get('Content-Type')
    if content_type not in ALLOWED_CONTENT_TYPE:
        return jsonify({'message': 'Unsupported media type'}), 415

    content_length = int(response.headers.get('Content-Length', 0))
    if content_length > MAX_CONTENT_SIZE:
        return jsonify({'message': 'Image too large'}), 400

    # Save image
    image_path = os.path.join('images', f'{os.path.basename(url)}')
    with open(image_path, 'wb') as f:
        for chunk in response.iter_content(chunk_size=8192):
            f.write(chunk)

    return jsonify({'message': 'Image fetched and saved successfully'}), 200

if __name__ == '__main__':
    # app.run(ssl_context=('path/to/cert.pem', 'path/to/key.pem'),debug=False, host='0.0.0.0', port=9879)
    app.run(host='0.0.0.0', port=9879)
```
* No proper validation for image uploads
* No file validation , as a vulnerable file can be easily injectable 
* wrong `SSL Context` used