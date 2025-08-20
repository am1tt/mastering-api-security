---
created: 2025-08-20 T12:57
---
---




## Summary 
---
> as done previously as well , BFLA means when API does not check who is *Authorized* to access a endpoint , where one should not have access to

* leading to `privilage escalation`
* partial use 
* account highjack 
* Financial Loss and more 



## Code  
---
```
from flask import Flask, request, jsonify, redirect, url_for
from flask_login import LoginManager, UserMixin, login_user, login_required, current_user
from flasgger import Swagger, swag_from

app = Flask(__name__)
app.secret_key = 'supersecretkey'
swagger = Swagger(app)

login_manager = LoginManager()
login_manager.init_app(app)

class User(UserMixin):
    def __init__(self, id, role):
        self.id = id
        self.role = role

# Example users
users = {
    1: User(1, "admin"),
    2: User(2, "user"),
}

@login_manager.user_loader
def load_user(user_id):
    return users.get(int(user_id))

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        user_id = request.form.get('user_id')
        user = users.get(int(user_id))
        if user:
            login_user(user)
            return redirect(url_for('get_users'))
    return '''
    <form method="POST">
        User ID: <input type="text" name="user_id"><br>
        <input type="submit" value="Login">
    </form>
    '''

@app.route('/api/admin/v1/users/all', methods=['GET'])
@login_required
@swag_from({
    'responses': {
        200: {
            'description': 'A list of all users.',
            'schema': {
                'type': 'array',
                'items': {
                    'type': 'object',
                    'properties': {
                        'id': {'type': 'integer'},
                        'role': {'type': 'string'}
                    }
                }
            }
        }
    },
    'security': [
        {
            'basicAuth': []
        }
    ],
    'tags': ['Admin']
})
def get_users():
    users_list = [{"id": user.id, "role": user.role} for user in users.values()]
    return jsonify(users_list)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=9877)
```

### Above code summarized 
---
- summarized that the `BFLA` , and demonstrated the authorized login through id
   
- at endpoint `/login` , consisted of `admin` and `user` login 
  
- and if someone with different userId such that `3` would send request for login it would block it as *Unauthorized* means the Authorization is properly implemented 
  
- however still some validations needs to implemented in code , but as its a lab , so no problem with that for now 