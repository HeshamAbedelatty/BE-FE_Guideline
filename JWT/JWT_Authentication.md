# JWT Authentication Guide
This Blog provides a step-by-step guide to understanding, implementing, and testing JWT in both frontend and backend scenarios.

## **Table of Contents**
1. [Introduction to JWT](##introduction-to-jwt)
2. [How JWT Works](#how-jwt-works)
3. [JWT Structure](#jwt-structure)
    - [Access Tokens](#access-tokens)
    - [Refresh Tokens](#refresh-tokens)
4. [Frontend Guide](#frontend-guide)
    - [How to Handle JWT in Frontend](#how-to-handle-jwt-in-frontend)
    - [Manual Token Handling Examples](#manual-token-handling-examples)
    - [Built-in Library Examples](#built-in-library-examples)
5. [Backend Guide](#backend-guide)
    - [Trying JWT in Postman](#trying-jwt-in-postman)

---

## **1. Introduction to JWT**
<a id="introduction-to-jwt"></a>
**JWT (JSON Web Token)** is a compact, URL-safe token used for securely transmitting information between parties as a JSON object. It is commonly used for authentication, where a client receives a token after a successful login and then includes this token in subsequent requests to protected endpoints.

---

## **2. How JWT Works**
<a id="how-jwt-works"></a>

JWT follows a simple flow:
1. **Login**: The user logs in with credentials (e.g., email/password).
2. **Token Issuance**: The server validates the credentials and issues a JWT, which is sent back to the client.
3. **Using the Token**: The client includes the JWT in the Authorization header for future API requests, allowing the server to identify the user without requiring them to re-authenticate.
4. **Token Expiry**: JWTs have a limited lifespan. When the token expires, the client either refreshes it using a refresh token or the user logs in again.

---

## **3. JWT Structure**
<a id="jwt-structure"></a>

JWT consists of three parts, separated by dots (`.`):
1. **Header**: Contains metadata about the type of token and the algorithm used for signing.
2. **Payload**: Contains the claims or information being transmitted, such as user data and the expiration time (`exp`).
3. **Signature**: Verifies the authenticity of the token, ensuring it has not been tampered with.

### **Access Tokens**
<a id="access-tokens"></a>
- **Short-lived** (e.g., 5-15 minutes).
- Used to access protected resources.
- Stored client-side, typically in local storage or cookies.

### **Refresh Tokens**
<a id="refresh-tokens"></a>
- **Long-lived** (e.g., days to weeks).
- Used to obtain a new access token when the current one expires.
- Usually stored securely (e.g., in `httpOnly` cookies).

---

## **4. Frontend Guide**
<a id="frontend-guide"></a>

### **How to Handle JWT in Frontend**
<a id="how-to-handle-jwt-in-frontend"></a>

### **Manual Token Handling**
<a id="manual-token-handling-examples"></a>

You can manually handle JWTs by storing them in local storage or cookies and sending them in the Authorization header.

#### **Example: Storing and Using JWT**

```javascript
// Store JWT after login
localStorage.setItem('access_token', jwtToken);

// Add JWT to Authorization header in API requests
fetch('https://api.yoursite.com/protected', {
  method: 'GET',
  headers: {
    'Authorization': `Bearer ${localStorage.getItem('access_token')}`
  }
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

### **Handling Expired Tokens**

```javascript
function isTokenExpired(token) {
  const payload = JSON.parse(atob(token.split('.')[1]));
  const currentTime = Math.floor(Date.now() / 1000);
  return payload.exp < currentTime;
}

if (isTokenExpired(localStorage.getItem('access_token'))) {
  console.log('Token expired. Refresh needed.');
}
```

### **Built-in Libraries**
<a id="built-in-library-examples">

You can use libraries like `jwt-decode` for easier token handling.

#### **Example: Using `jwt-decode` Library**

```bash
npm install jwt-decode
```

```javascript
import jwt_decode from 'jwt-decode';

const decoded = jwt_decode(localStorage.getItem('access_token'));
console.log(decoded);
```

---

## **5. Backend Guide**
<a id="backend-guide"></a>

### **Trying JWT in Postman**
<a id="trying-jwt-in-postman"></a>

#### **Login and Retrieve Token**
1. Open Postman and create a POST request to your login endpoint (e.g., `/api/token/`).
2. Include the user's credentials in the request body:
   ```json
   {
     "username": "user@example.com",
     "password": "password123"
   }
   ```
3. Send the request and retrieve the access and refresh tokens from the response.

![Postman Login](https://i.imgur.com/POSTMAN_LOGIN_EXAMPLE.png)

#### **Using the Access Token**

1. Create a new GET request in Postman to a protected route (e.g., `/api/protected/`).
2. Add the Authorization header with the access token:
   ```
   Authorization: Bearer <your_access_token>
   ```

![Postman Authorization](https://i.imgur.com/POSTMAN_AUTHORIZATION_EXAMPLE.png)

3. Send the request to access the protected resource.

![Postman Success](https://i.imgur.com/POSTMAN_SUCCESS_EXAMPLE.png)

#### **Refreshing the Token**

1. Create a new POST request to the refresh endpoint (e.g., `/api/token/refresh/`).
2. Add the refresh token to the request body:
   ```json
   {
     "refresh": "<your_refresh_token>"
   }
   ```

3. Send the request to get a new access token.

![Postman Refresh](https://i.imgur.com/POSTMAN_REFRESH_EXAMPLE.png)

---
