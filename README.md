# BE-FE_Guideline 



# API Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [Authentication](#authentication)
3. [User Registration](#user-registration)
4. [User Endpoints](#user-endpoints)
   - [Create User](#create-user)
   - [Retrieve User](#retrieve-user)
   - [Update User](#update-user)
   - [Delete User](#delete-user)
5. [Other Endpoints](#other-endpoints)

## Introduction
This API provides endpoints to manage users and other resources within the application. The API is built using Django REST Framework and follows RESTful principles.

## Authentication
Authentication is required for most endpoints, except for user registration and login. Ensure that you include an `Authorization: Bearer <token>` header in your requests where authentication is required.

## User Registration

### `POST /api/register/`
Register a new user.

**Request:**
```json
POST /api/register/
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "Password123!",
  "profile_image": "path/to/image.jpg"
}
```

**Response:**
```json
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "image_url": "http://yourdomain.com/media/path/to/image.jpg"
}
```

## User Endpoints

### `POST /api/users/`
Create a new user.

**Request:**
```json
POST /api/users/
Content-Type: application/json

{
  "username": "jane_doe",
  "email": "jane@example.com",
  "password": "Password123!",
  "profile_image": "path/to/image.jpg"
}
```

**Response:**
```json
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": 2,
  "username": "jane_doe",
  "email": "jane@example.com",
  "image_url": "http://yourdomain.com/media/path/to/image.jpg"
}
```

### `GET /api/users/{id}/`
Retrieve a user's information by their ID.

**Request:**
```http
GET /api/users/1/
Authorization: Bearer <token>
```

**Response:**
```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "image_url": "http://yourdomain.com/media/path/to/image.jpg"
}
```

### `PUT /api/users/{id}/`
Update an existing user's information.

PUT /api/users/1/
Authorization: Bearer <token>
Content-Type: application/json

**Request:**
```json

{
  "username": "john_updated",
  "email": "john_updated@example.com",
  "profile_image": "path/to/new_image.jpg"
}
```

**Response:**
```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 1,
  "username": "john_updated",
  "email": "john_updated@example.com",
  "image_url": "http://yourdomain.com/media/path/to/new_image.jpg"
}
```

### `DELETE /api/users/{id}/`
Delete a user by their ID.

**Request:**
```http
DELETE /api/users/1/
Authorization: Bearer <token>
```

**Response:**
```json
HTTP/1.1 204 No Content
```

## Other Endpoints
Additional endpoints for other resources (e.g., posts, comments, etc.) can follow a similar structure as the user endpoints.

---

**Note:** Replace `http://yourdomain.com` with your actual domain name when deploying to production.
```

### How to Use This README

- **Introduction:** A brief overview of the API and its purpose.
- **Authentication:** Instructions on how to authenticate API requests.
- **User Registration:** An example of how to register a new user using the `/api/register/` endpoint.
- **User Endpoints:** Examples of how to create, retrieve, update, and delete users.
- **Other Endpoints:** You can expand this section to include examples of other resources in your application (e.g., posts, comments).

### Customization
You can customize this template to fit your specific project. For example, if you have additional endpoints for managing other resources like posts, comments, or groups, you can add those under the "Other Endpoints" section.
