---
created: 2025-08-12T13:48:00
---
----



## Run the code 
---
```
from flask import Flask, jsonify, request

  

app = Flask(__name__)

  

# Sample data

users = [

    {"id": 1, "username": "Alice", "documents": [{"id": "doc1", "content": "Alice's Document 1"}]},

    {"id": 2, "username": "Bob", "documents": [{"id": "doc2", "content": "Bob's Document 1"}]},

]

  
  

def find_user_by_id(user_id):

    for user in users:

        if user["id"] == user_id:

            return user

    return None

  
  

def find_document_by_id(document_id):

    for user in users:

        for document in user["documents"]:

            if document["id"] == document_id:

                return document

    return None

  
  

@app.route("/api/users/<int:user_id>/documents", methods=["GET"])

def get_user_documents(user_id):

    user = find_user_by_id(user_id)

    if user:

        return jsonify(user["documents"])

    return jsonify({"error": "User not found"}), 404

  
  

@app.route("/api/documents/<document_id>", methods=["DELETE"])

def delete_document(document_id):

    document = find_document_by_id(document_id)

    if document:

        # BOLA vulnerability - No authorization check for deleting the document

        users[0]["documents"].remove(document)

        return jsonify({"message": "Document deleted successfully"})

    return jsonify({"error": "Document not found"}), 404

  

# make an endpoint to create a document

@app.route("/api/users/<int:user_id>/documents", methods=["POST"])

def create_document(user_id):

    user = find_user_by_id(user_id)

    if user:

        # BOLA vulnerability - No authorization check for creating the document

        new_document = {"id": "doc3", "content": "Alice's Document 3"}

        users[0]["documents"].append(new_document)

        return jsonify({"message": "Document created successfully"})

    return jsonify({"error": "User not found"}), 404

  

# make an endpoint to update a document

@app.route("/api/documents/<document_id>", methods=["PUT"])

def update_document(document_id):

    document = find_document_by_id(document_id)

    if document:

        # BOLA vulnerability - No authorization check for updating the document

        document["content"] = "Alice's Document 1 - Updated"

        return jsonify({"message": "Document updated successfully"})

    return jsonify({"error": "Document not found"}), 404

  
  
  

if __name__ == "__main__":

    app.run(host='0.0.0.0', port=9873)
```


## 💁‍♂️ What i did  
---
* I tested the endpoints from the postman 
* such as deleting 
	* `http://http://127.0.0.1:9873/api/documents/doc1` 
* and getting data from 
	* `http://http://127.0.0.1:9873/api/users/1/documents`
* and also tried for access gaining , however as there is a validation for that in code , i did not proceeded further
* the above experiences and learnings , made me understand BOLA more as mentioned about identifiers and objects from the API