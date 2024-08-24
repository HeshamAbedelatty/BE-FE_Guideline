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
3. **Using the Token**: The client includes the JWT in the Authorization header for future API requests, **allowing** the server to identify the user without requiring them to **re-authenticate**.
4. **Token Expiry**: JWTs have a limited lifespan. When the token expires, the client either refreshes it using a refresh token or the user logs in again.

---

## **3. JWT Structure**
<a id="jwt-structure"></a>

JWT consists of three parts, separated by dots (`.`):
1. **Header**:
   - **Purpose**: Specifies the token type and the signing algorithm used.
   - **Example**: `{ "alg": "HS256", "typ": "JWT" }`
   - **Encoded**: Base64Url encoding.

2. **Payload**:
   - **Purpose**: Contains the claims, which are statements about an entity (usually the user) and additional metadata.
   - **Example**: `{ "sub": "1234567890", "name": "John Doe", "iat": 1516239022 }`
   - **Encoded**: Base64Url encoding.
   - **JWT Date Representation**:
        - `iat` (issued at) represents the time when the token was issued.
        - `1516239022` is a Unix timestamp.
        - The Unix timestamp `1516239022` is converted to a human-readable date `2018-01-18 21:43:42` 

3. **Signature**:
   - **Purpose**: Ensures the integrity and authenticity of the token by verifying that the header and payload were not altered.
   - **How it's created**: By combining the encoded header and payload with a secret key and then hashing it.
   - **Example**: `SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

### Example JWT

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ
.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
- **Header**: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`
- **Payload**: `eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ`
- **Signature**: `SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`
  
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

### **JWT Authentication Setup**

Ensure you have `djangorestframework-simplejwt` installed and configured:

**Install package:**

```bash
pip install djangorestframework-simplejwt
```

**Configure in `settings.py`:**

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
}

# Add JWT settings
from datetime import timedelta

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=60),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=24),
    'ROTATE_REFRESH_TOKENS': True,
    'BLACKLIST_AFTER_ROTATION': True,
}
```

#### **2. Refresh Token Endpoint**

**urls.py:**

```python
from django.urls import path
from rest_framework_simplejwt import views as jwt_views

urlpatterns = [
    # Other paths...
    path('api/token/', jwt_views.TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/token/refresh/', jwt_views.TokenRefreshView.as_view(), name='token_refresh'),
]
```

**Views Handling JWT:**

***You typically don’t need to manually handle JWT in views as `simplejwt` provides the required endpoints. However, you may customize them if needed.***

#### **3. Protecting Endpoints**

Use the JWT authentication class to protect your API views.

**views.py:**

```python
from rest_framework import permissions
from rest_framework.views import APIView
from rest_framework.response import Response

class ProtectedView(APIView):
    permission_classes = [permissions.IsAuthenticated] # Check the Key if it in request or not.

    def get(self, request):
        user = request.user
        return Response({'message': 'You have access to this view!', 'user': user.username})
```


### **Trying JWT in Postman**
<a id="trying-jwt-in-postman"></a>

#### **Login and Retrieve Token**
1. **POST Request to Login Endpoint**

   - **URL**: `/api/token/` or `/login`
   - **Method**: POST
   - **Body**:
     ```json
     {
       "username": "user@example.com",
       "password": "password123"
     }
     ```
   - **Response**: 
     ```json
     {
       "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
       "refresh": "dGhpcy1pcy1hLXJlZnJlc2g2MDY1ZGZkY2FzbmJhdHM5ZDMzZTEz"
     }
     ```

   ![Postman Login](POSTMAN_LOGIN_EXAMPLE.jpg)

---

#### **Using the Access Token**
1. **GET Request to books root**

   - **URL**: `/List/`
   - **Method**: GET
   - **Headers**:
     ```
     Authorization: Bearer <your_access_token>
     ```
   - **Response**:
     ```json
     {
       "data": "This is my lists data"
     }
     ```

   ![Postman Authorization](POSTMAN_AUTHORIZATION_get.jpg)
   ![Postman Authorization](POSTMAN_AUTHORIZATION_get2.jpg)

---

#### **Refreshing the Token**
1. **POST Request to Refresh Endpoint**

   - **URL**: `/api/token/refresh/`
   - **Method**: POST
   - **Body**:
     ```json
     {
       "refresh": "dGhpcy1pcy1hLXJlZnJlc2g2MDY1ZGZkY2FzbmJhdHM5ZDMzZTEz"
     }
     ```
   - **Response**:
     ```json
     {
       "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
     }
     ```
---
