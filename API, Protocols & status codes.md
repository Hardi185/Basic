# Table of Contents

1. [What is an API?](#what-is-an-api)
    - [Stateless vs Stateful APIs](#Stateless-vs-Stateful-APIs)
    - [Types of APIs](#types-of-apis)
    - [Different API Protocols](#different-api-protocols)
    
2. [HTTP vs HTTPS - Key Differences & Security](#http-vs-https-key-differences-security)

3. [Common HTTP Response Status Codes Returned by APIs](#common-http-response-status-codes-returned-by-apis)


-------------------------------------------------------------------------------------

## What is an API?

API (Application Programming Interface) is a set of rules and protocols that allow different software applications to communicate with each other. 
```yaml
Think of it like a intermediate between client & server.
Takes request sends to server, takes the response sends to client.
```

## What data is sent as request and what will return as response:

### API Request:
**1. HTTP Method:** GET, POST, PUT, PATCH, DELETE

**2. Endpoint (URL):**
````yaml
/api/users
/api/users/10
````

**3. Headers:**
````yaml
Content-Type: application/json
Authorization: Bearer <token>
Accept: application/json
````
you could say:
````yaml
Content-Type → what I send
Accept → what I want back
Authorization → who I am
````

**4. Authentication (via headers or cookies):**
- Authentication information can be sent either as:

🔹 JWT / Token-based (Stateless)
````yaml
Authorization: Bearer <JWT>
````

🔹 Cookie-based (Usually Stateful)
````yaml
Cookie: JSESSIONID=abc123
````
🔑 Cookies can also carry JWTs, in which case the API is still stateless.

**5. Request Body (optional):**
```yaml
{
  "name": "Nandini",
  "email": "nandini@gmail.com",
  "role": "software_engineer"
}
````

### API Reponse:
- Code - meaning, Headers, Response body
```yaml
201 Created
Content-Type: application/json
{
  "id": 10,
  "name": "Nandini",
  "email": "nandini@gmail.com"
}
````


--------
## Stateless vs Stateful APIs
- APIs can be either stateless or stateful, depending on how they manage client-server interactions.

1️⃣ Stateless APIs (Most Common)

- The server does not store any client-specific session data.
- Each request from the client must contain all necessary information (e.g., authentication tokens, parameters).
- Example: REST APIs (Representational State Transfer) are stateless.
- Benefits:

  ✔ Scalable, as each request is independent.
  
  ✔ Easier to cache responses.
  
  ✔ More resilient to failures.

- Example of a Stateless API (RESTful API Request)

````yaml
GET /user/123
Authorization: Bearer <token>
````

- The server processes this request without remembering past interactions.

2️⃣ Stateful APIs

- The server stores client session data between requests.
- The client must maintain the same session (e.g., cookies, session IDs).
- Example: SOAP APIs (Simple Object Access Protocol) and WebSockets.
- Use cases: Banking transactions, real-time applications, multi-step processes.
- Example of a Stateful API (Session-Based Login)

````yaml
POST /login
{
  "username": "user123",
  "password": "mypassword"
}
````

- The server responds with a session ID that must be used in future requests.


## Types of APIs

- REST API (Representational State Transfer)
  - Uses HTTP methods (GET, POST, PUT, DELETE).
  - Works with JSON or XML data.
  - Stateless, meaning each request is independent.
  - Example: Fetching user data from a web service.
    
- SOAP API (Simple Object Access Protocol)

  - Uses XML-based messaging format.
  - More secure and reliable than REST.
  - Requires more bandwidth and is slower than REST.
  - Used in banking and enterprise applications.

- GraphQL API

  - Allows clients to request only the data they need.
  - More efficient than REST (reduces over-fetching of data).
  - Used by Facebook, GitHub, and modern web apps.

- WebSocket API

  - Provides real-time communication (full-duplex).
  - Used for chat applications, live stock updates, gaming, etc.
  - Faster than REST for real-time data transfer.

## Different API Protocols

![Protocol](https://github.com/user-attachments/assets/e0091b70-d47e-465e-beb4-4fb67871da57)

---------------------------------------------------------------------------------------------------------

## 🌐 HTTP vs HTTPS - Key Differences & Security
<a name="http-vs-https-key-differences-security"></a>


The main difference between **HTTP (HyperText Transfer Protocol)** and **HTTPS (HyperText Transfer Protocol Secure)** is **security**. HTTPS provides encrypted communication using **SSL/TLS**, making it more secure than HTTP.


### 🔹 Key Differences Between HTTP and HTTPS

| Feature          | HTTP  | HTTPS  |
|-----------------|------|--------|
| **Security**    | ❌ Not secure | ✅ Secure (uses SSL/TLS encryption) |
| **Data Encryption** | ❌ No encryption (plain text data) | ✅ Encrypted with SSL/TLS |
| **Data Integrity** | ❌ Can be modified (MITM attacks) | ✅ Prevents data tampering |
| **Authentication** | ❌ No identity verification | ✅ Uses SSL certificates for verification |
| **Port Used** | 80 | 443 |
| **Performance** | ⚡ Faster (no encryption overhead) | 🛡 Slightly slower due to encryption but optimized with HTTP/2 |


### 🔹 How HTTPS is More Secure?

#### 🔒 **Encryption**  
HTTPS encrypts data using **SSL/TLS**, preventing hackers from stealing sensitive information (e.g., passwords, credit card details).  

#### 🛡 **Data Integrity**  
- Ensures that data is not altered or corrupted during transfer.  
- Detects if someone attempts to modify data in transit.  

#### ✅ **Authentication**  
- Uses **SSL certificates** to verify the website’s authenticity.  
- Prevents phishing attacks by ensuring users connect to the correct server.  

#### 🌍 **SEO & Trust**  
- Google ranks HTTPS websites **higher** in search results.  
- Users trust HTTPS sites more (padlock icon 🔒 in the browser).  


### 🔹 When to Use HTTP vs. HTTPS?

✔ **Use HTTP** for **public, non-sensitive content** (e.g., blogs, news).  
✔ **Use HTTPS** for **secure communication** (e.g., login pages, payment processing, confidential data transfer).  

--------------------------------------------------------------------------------------------------------------------------------------------------------

## Common HTTP Response Status Codes Returned by APIs

When an API responds to a request, it includes an HTTP status code to indicate the outcome. Here are the most commonly used response codes:

### ✅ 1xx – Informational Responses (Rarely Used)
| Code | Meaning | Description |
|------|---------|-------------|
| **100** | Continue | Server received request headers and client should proceed. |
| **101** | Switching Protocols | Server is switching protocols (e.g., HTTP to WebSocket). |

### ✅ 2xx – Success Responses
| Code | Meaning | Description |
|------|---------|-------------|
| **200** | OK | Request was successful, and response contains data. |
| **201** | Created | Resource was successfully created (e.g., after `POST`). |
| **202** | Accepted | Request was accepted for processing but not yet completed. |
| **204** | No Content | Request succeeded, but no response body is returned. |

### ⚠️ 3xx – Redirection Responses
| Code | Meaning | Description |
|------|---------|-------------|
| **301** | Moved Permanently | Resource URL has changed permanently. |
| **302** | Found | Temporary redirection to another URL. |
| **304** | Not Modified | Resource hasn’t changed (used for caching). |

### ⛔ 4xx – Client Errors
| Code | Meaning | Description |
|------|---------|-------------|
| **400** | Bad Request | Request is invalid (e.g., missing parameters, incorrect format). |
| **401** | Unauthorized | Authentication is required but missing or invalid. |
| **403** | Forbidden | Client is authenticated but doesn’t have permission. |
| **404** | Not Found | Requested resource doesn’t exist. |
| **405** | Method Not Allowed | HTTP method (`GET`, `POST`, etc.) is not allowed on this endpoint. |
| **409** | Conflict | Request conflicts with the current state (e.g., duplicate resource). |
| **429** | Too Many Requests | Rate limiting: Client has sent too many requests. |

### ❌ 5xx – Server Errors
| Code | Meaning | Description |
|------|---------|-------------|
| **500** | Internal Server Error | Generic error when something goes wrong on the server. |
| **502** | Bad Gateway | Server received an invalid response from an upstream service. |
| **503** | Service Unavailable | Server is temporarily unavailable (e.g., overloaded, maintenance). |
| **504** | Gateway Timeout | Server didn’t receive a response from an upstream service in time. |

-----------
## How to Improve API perfomance:
<img width="671" height="642" alt="image" src="https://github.com/user-attachments/assets/9d97f2ad-8df5-4ba2-b5ce-417c13c8d404" />
