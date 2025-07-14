---

---
2025-06-23 19:32

---


## 💤REST (**Representional state transfer**)
---
* Defination & Guiding Principal
* Stateless Interactions
* Client - Server *Architechture*
* Cachable (information)
* Layered System
* Uniform Interface (using ==HTTP methods==)
---
```
Request

POST /stocks HTTP/1.1

Host: www.example.com

Content: application/json

{

  “stock”: “AAPL”,

  “price”:  “145.00”

}

Response

{

“status”: “success”,

“message”: “Stock added successfully”

}
```

> here as you can see the response or the content-type consist of *==JSON==*


|                                                                                                                                                                                            |                                                                                                                                                         |     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| where the client side handles <br>the storage of the data and note <br>the server , this achieves *Scalibility*<br>and fast request and response on <br>server , eg : Cache Storage (REST) | Where the data or any data is stored<br>on the server only , and not on the client <br>side , allowing faster processing and <br>improved performance , |     |

----

## ⚙️Components of **REST**
---
* Resources (Identified by URL)
* HTTP Methods (GET,POST,PUT,DELETE)
* Status Codes
* MIME Types (JSON , XML)
---

