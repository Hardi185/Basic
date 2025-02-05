# What is an API?

API (Application Programming Interface) is a set of rules and protocols that allow different software applications to communicate with each other. APIs define the methods and data formats that applications can use to request and exchange information.

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


![Protocol](https://github.com/user-attachments/assets/e0091b70-d47e-465e-beb4-4fb67871da57)

---------------------------------------------------------------------------------------------------------

The main difference between HTTP (HyperText Transfer Protocol) and HTTPS (HyperText Transfer Protocol Secure) is security.

🔹 Key Differences Between HTTP and HTTPS

- Feature	HTTP	HTTPS
- Security	Not secure	Secure (uses SSL/TLS encryption)
- Data Encryption	No encryption (data is sent as plain text)	Uses SSL/TLS to encrypt data
- Data Integrity	Data can be modified by attackers (MITM attacks)	Prevents data tampering
- Authentication	No identity verification	Uses SSL certificates to verify server identity
- Port Used	Port 80	Port 443
Performance	Faster (no encryption overhead)	Slightly slower due to encryption but optimized with HTTP/2
🔹 How HTTPS is More Secure?
Encryption 🔒

HTTPS encrypts data using SSL/TLS, preventing hackers from stealing sensitive information (passwords, credit card details, etc.).
Data Integrity 🛡

Ensures that data is not altered or corrupted during transfer.
Detects if someone tries to modify the data in transit.
Authentication ✅

Uses SSL certificates to verify the website's authenticity.
Helps prevent phishing attacks by ensuring users connect to the correct server.
SEO and Trust 🌍

Google ranks HTTPS websites higher in search results.
Users trust HTTPS sites more (padlock icon in browser).
When to Use HTTP vs. HTTPS?
✔ Use HTTP for public, non-sensitive content (e.g., blogs, news).
✔ Use HTTPS for secure communication (e.g., login pages, payment sites, confidential data transfer).
