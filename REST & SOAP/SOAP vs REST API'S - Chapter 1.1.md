2025-06-20 19:53

---


## 🫧 SOAP ( **Simple Object Access Protocol** )
---
- Defination & Primary function
- ==XML - based message *protocol*==
- Protocol Independence 

> can be used over *smpt , ftp , http, etc*

-  built in error handlings 
- stateful operations
---

## 😎SOAP Request **Example**

> Request : 

```
POST/InStock HTTP/1.1
HOST www.ab.com
Content-Type:application/soap+xml;charset=utf-8
Content-length:20
```

if you noticed it in ==content-type== sometimes there is also application/*json* which refers to the REST Request's one

---

another example can be : 

```
<?xml version="1.0">
<soap.header>
<soap.body>
</soap.envelope>
```

---
## ⚙️Components of **SOAP**
---
* `Envelope` : defines message from start to the end eg : `body`
* `Header` : contains attributes like authentication or *session information*
* `Body` : contains the actual request and response of the information
* `Fault` : Error handling information (if a problem occurs) 
---

1 : Communication method
2 : Message Format 
3 : Statefullness
4 : Error handling
5 : Performance Consideration
6 : Flexibility and ease of use

|     | SOAP                                       | REST                                                                |
| --- | ------------------------------------------ | ------------------------------------------------------------------- |
|     | Service Operations                         | CRUD Operations                                                     |
|     | uses XML                                   | use JSON \| XML                                                     |
|     | can be Statefull or statless               | is Stateless                                                        |
|     | has inbuilt error handling                 | done via HTTP methods                                               |
|     | due to XML parsing (takes <br>longer time) | as stateless , sessions are less present making in longer processes |
|     |                                            |                                                                     |

---
## 👥Use Cases
---
* Soap : use legacy systems , containing ACID complaince
* REST : modern web applications and mobile applications , public API'S 
  for third party implementation , situations that can benefit from CACHING
---

|Feature|SOAP|REST|
|---|---|---|
|**Type**|Protocol|Architectural Style|
|**Data Format**|XML **only**|JSON (mostly), XML, etc.|
|**Transport**|Can use HTTP, SMTP, etc.|HTTP only|
|**State**|Can be **stateful**|**Stateless**|
|**Security**|Built-in via WS-Security|HTTPS + custom auth (e.g., OAuth)|
|**Use Case**|Banking, enterprise apps|Public APIs, web/mobile apps|
|**Complexity**|More verbose and strict|Simpler, faster to integrate|
