# What is an API?

API (Application Programming Interface) is a set of rules and protocols that allow different software applications to communicate with each other. 
```yaml
Think of it like a intermediate between client & server.
```

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

