# IAM Service API Documentation

**Version:** 1.0.0  
**Base URL:** `http://localhost:8080/iam`  
**Contact:** Healthcare Team (team@healthcare.com)

## Overview

API documentation for IAM microservice providing user management, role-based access control, and authentication services.

## Authentication

This API uses Bearer Token authentication. Include the following header in your requests:

```
Authorization: Bearer <your_jwt_token>
```

## Table of Contents

- [Authentication Endpoints](#authentication-endpoints)
- [User Management](#user-management)
- [Role Management](#role-management)
- [Privilege Management](#privilege-management)
- [Data Models](#data-models)

---

## Authentication Endpoints

### POST /auth/register
Register a new user account.

**Request Body:**
```json
{
  "fullName": "string",
  "email": "string",
  "phone": "string",
  "gender": "string",
  "date_of_Birth": "string",
  "address": "string",
  "password": "string",
  "identifyNum": "string"
}
```

**Response:** `200 OK`
```json
{
  "message": "User registered successfully"
}
```

### POST /auth/login
Authenticate user and receive access token.

**Request Body:**
```json
{
  "username": "string",
  "password": "string"
}
```

**Response:** `200 OK`
```json
{
  "token": "string",
  "user": "object"
}
```

### POST /auth/logout
Logout current user session.

**Response:** `200 OK`
```json
{
  "message": "Logged out successfully"
}
```

### POST /auth/me
Get current authenticated user information.

**Request Body:**
```json
{
  "username": "string",
  "password": "string"
}
```

**Response:** `200 OK`
```json
{
  "user": "object"
}
```

### POST /auth/forgot-password
Request password reset for user account.

**Request Body:**
```json
"user@example.com"
```

**Response:** `200 OK`
```json
{
  "message": "Password reset email sent"
}
```

### POST /auth/reset-password
Reset user password using token.

**Request Body:**
```json
{
  "email": "string",
  "newPassword": "string"
}
```

**Constraints:**
- `email`: Required
- `newPassword`: Required, minimum 8 characters

**Response:** `200 OK`
```json
{
  "message": "Password reset successfully"
}
```

### POST /auth/verify-otp
Verify OTP for authentication.

**Request Body:**
```json
{
  "email": "string",
  "otp": "string"
}
```

**Response:** `200 OK`
```json
{
  "verified": true
}
```

### POST /auth/verify-reset-otp
Verify OTP for password reset.

**Request Body:**
```json
{
  "email": "string",
  "otp": "string"
}
```

**Response:** `200 OK`
```json
{
  "verified": true
}
```

### POST /auth/verify-activation-otp
Verify OTP for account activation.

**Request Body:**
```json
{
  "email": "string",
  "otp": "string"
}
```

**Response:** `200 OK`
```json
{
  "verified": true
}
```

### POST /auth/resend-otp
Resend OTP to user email.

**Request Body:**
```json
{
  "email": "string"
}
```

**Response:** `200 OK`
```json
{
  "message": "OTP sent successfully"
}
```

### GET /auth/activation/{email}
Activate user account via email.

**Parameters:**
- `email` (path, required): User email address

**Response:** `200 OK`
```json
{
  "message": "Account activated successfully"
}
```

---

## User Management

### POST /users
Create a new user.

**Request Body:**
```json
{
  "fullName": "string",
  "email": "string",
  "phone": "string",
  "identifyNum": "string",
  "address": "string",
  "gender": "string",
  "password": "string",
  "date_of_Birth": "string",
  "roleIds": [0]
}
```

**Required Fields:** `email`, `fullName`, `identifyNum`, `phone`

**Response:** `200 OK`
```json
{
  "id": 1,
  "message": "User created successfully"
}
```

### GET /users/{id}
Get user by ID.

**Parameters:**
- `id` (path, required): User ID (integer)

**Response:** `200 OK`
```json
{
  "fullName": "string",
  "email": "string",
  "phone": "string",
  "identifyNum": "string",
  "gender": "string",
  "address": "string",
  "date_of_Birth": "string",
  "status": "string",
  "roleIds": [0],
  "updateAt": "2025-08-05T10:30:00Z",
  "updated_by_email": "string"
}
```

### PUT /users/{id}
Update user information.

**Parameters:**
- `id` (path, required): User ID (integer)

**Request Body:**
```json
{
  "fullName": "string",
  "date_of_Birth": "string",
  "email": "string",
  "address": "string",
  "gender": "string",
  "phone": "string",
  "status": "string",
  "identifyNum": "string",
  "roleIds": [0]
}
```

**Required Fields:** `address`, `date_of_Birth`, `email`, `fullName`, `gender`, `identifyNum`, `phone`, `status`

**Response:** `200 OK`
```json
{
  "fullName": "string",
  "email": "string",
  "phone": "string",
  "identifyNum": "string",
  "gender": "string",
  "address": "string",
  "date_of_Birth": "string",
  "status": "string",
  "roleIds": [0],
  "updateAt": "2025-08-05T10:30:00Z",
  "updated_by_email": "string"
}
```

### DELETE /users/{id}
Delete user by ID.

**Parameters:**
- `id` (path, required): User ID (integer)

**Response:** `200 OK`
```json
{
  "message": "User deleted successfully"
}
```

### PUT /users/{userId}/change-password
Change user password.

**Parameters:**
- `userId` (path, required): User ID (integer)

**Request Body:**
```json
{
  "oldPassword": "string",
  "newPassword": "string"
}
```

**Required Fields:** `newPassword`, `oldPassword`

**Response:** `200 OK`
```json
"Password changed successfully"
```

### PATCH /users/{id}/status
Update user status (lock/unlock).

**Parameters:**
- `id` (path, required): User ID (integer)

**Request Body:**
```json
{
  "lock": true
}
```

**Response:** `200 OK`
```json
{
  "message": "User status updated"
}
```

### POST /users/upload
Upload user profile picture.

**Request Body:**
```json
{
  "file": "binary"
}
```

**Required Fields:** `file`

**Response:** `200 OK`
```json
{
  "message": "Profile picture uploaded successfully"
}
```

### GET /users/search
Search users with pagination.

**Query Parameters:**
- `page` (integer, optional): Page number (default: 0)
- `size` (integer, optional): Page size (default: 5)
- `keyword` (string, optional): Search keyword
- `sortBy` (string, optional): Sort field (default: "fullName")
- `direction` (string, optional): Sort direction (default: "asc")

**Response:** `200 OK`
```json
{
  "roles": [
    {
      "fullName": "string",
      "email": "string",
      "phone": "string",
      "identifyNum": "string",
      "gender": "string",
      "address": "string",
      "date_of_Birth": "string"
    }
  ],
  "totalElements": 0,
  "totalPages": 0,
  "currentPage": 0,
  "pageSize": 0,
  "hasNext": true,
  "hasPrevious": true,
  "sortBy": "string",
  "sortDirection": "string",
  "empty": true,
  "message": "string"
}
```

### GET /users/filter
Filter users with advanced options.

**Query Parameters:**
- `searchText` (string, optional): Search text
- `sortBy` (string, optional): Sort field (default: "fullName")
- `direction` (string, optional): Sort direction (default: "asc")
- `offsetPage` (integer, optional): Offset page (default: 1)
- `limitOnePage` (integer, optional): Limit per page (default: 12)

**Response:** `200 OK`
```json
{
  "totalPages": 0,
  "totalElements": 0,
  "pageable": {
    "pageNumber": 0,
    "paged": true,
    "pageSize": 0,
    "unpaged": true,
    "offset": 0,
    "sort": {
      "sorted": true,
      "unsorted": true,
      "empty": true
    }
  },
  "content": [
    {
      "id": 0,
      "fullName": "string",
      "email": "string",
      "gender": "string",
      "status": "string",
      "phone": "string",
      "address": "string",
      "createdAt": "string",
      "updatedAt": "string",
      "roles": ["string"],
      "privileges": ["string"]
    }
  ]
}
```

---

## Role Management

### GET /roles
Get all roles.

**Response:** `200 OK`
```json
[
  {
    "roleId": 0,
    "name": "string",
    "description": "string",
    "code": "string",
    "created_at": "2025-08-05T10:30:00Z",
    "updated_at": "2025-08-05T10:30:00Z",
    "privileges": [
      {
        "privilegeId": 0,
        "code": "string",
        "description": "string"
      }
    ],
    "createdBy": {
      "userId": 0,
      "fullName": "string",
      "email": "string",
      "identifyNum": "string"
    },
    "updatedBy": {
      "userId": 0,
      "fullName": "string",
      "email": "string",
      "identifyNum": "string"
    }
  }
]
```

### POST /roles
Create a new role.

**Request Body:**
```json
{
  "roleId": 0,
  "code": "string",
  "name": "string",
  "description": "string",
  "privilegesIds": [0]
}
```

**Response:** `200 OK`
```json
{
  "roleId": 0,
  "code": "string",
  "name": "string",
  "description": "string",
  "privilegesIds": [0]
}
```

### GET /roles/{id}
Get role by ID.

**Parameters:**
- `id` (path, required): Role ID (integer)

**Response:** `200 OK`
```json
{
  "roleId": 0,
  "name": "string",
  "description": "string",
  "code": "string",
  "created_at": "2025-08-05T10:30:00Z",
  "updated_at": "2025-08-05T10:30:00Z",
  "privileges": [
    {
      "privilegeId": 0,
      "code": "string",
      "description": "string"
    }
  ],
  "createdBy": {
    "userId": 0,
    "fullName": "string",
    "email": "string",
    "identifyNum": "string"
  },
  "updatedBy": {
    "userId": 0,
    "fullName": "string",
    "email": "string",
    "identifyNum": "string"
  }
}
```

### PUT /roles/{id}
Update role by ID.

**Parameters:**
- `id` (path, required): Role ID (integer)

**Request Body:**
```json
{
  "roleId": 0,
  "code": "string",
  "name": "string",
  "description": "string",
  "privilegesIds": [0]
}
```

**Response:** `200 OK`
```json
{
  "roleId": 0,
  "code": "string",
  "name": "string",
  "description": "string",
  "privilegesIds": [0]
}
```

### DELETE /roles/{id}
Delete role by ID.

**Parameters:**
- `id` (path, required): Role ID (integer)

**Response:** `200 OK`

### GET /roles/paging
Get roles with pagination.

**Query Parameters:**
- `page` (integer, optional): Page number (default: 0)
- `size` (integer, optional): Page size (default: 5)
- `sortBy` (string, optional): Sort field (default: "id")
- `sortDirection` (string, optional): Sort direction (default: "asc")

**Response:** `200 OK`
```json
{
  "roles": [
    {
      "roleId": 0,
      "name": "string",
      "code": "string",
      "description": "string",
      "privileges": ["string"]
    }
  ],
  "totalElements": 0,
  "totalPages": 0,
  "currentPage": 0,
  "pageSize": 0,
  "hasNext": true,
  "hasPrevious": true,
  "sortBy": "string",
  "sortDirection": "string",
  "empty": true,
  "message": "string"
}
```

### POST /roles/filter
Search and filter roles.

**Request Body:**
```json
{
  "sort": "string",
  "filter": "string",
  "page_num": 0,
  "page_size": 0
}
```

**Response:** `200 OK`
```json
{
  "roles": [
    {
      "roleId": 0,
      "name": "string",
      "code": "string",
      "description": "string",
      "privileges": ["string"]
    }
  ],
  "totalElements": 0,
  "totalPages": 0,
  "currentPage": 0,
  "pageSize": 0,
  "hasNext": true,
  "hasPrevious": true,
  "sortBy": "string",
  "sortDirection": "string",
  "empty": true,
  "message": "string"
}
```

---

## Privilege Management

### GET /privileges
Get all privileges.

**Response:** `200 OK`
```json
[
  {
    "privilegeId": 0,
    "code": "string",
    "description": "string"
  }
]
```

### POST /privileges
Create a new privilege.

**Request Body:**
```json
{
  "privilegeId": 0,
  "code": "string",
  "description": "string"
}
```

**Response:** `200 OK`
```json
{
  "privilegeId": 0,
  "code": "string",
  "description": "string"
}
```

### GET /privileges/{id}
Get privilege by ID.

**Parameters:**
- `id` (path, required): Privilege ID (integer)

**Response:** `200 OK`
```json
{
  "privilegeId": 0,
  "code": "string",
  "description": "string"
}
```

### DELETE /privileges/{id}
Delete privilege by ID.

**Parameters:**
- `id` (path, required): Privilege ID (integer)

**Response:** `200 OK`

---

## Data Models

### ChangePasswordRequest
```json
{
  "oldPassword": "string",
  "newPassword": "string"
}
```
**Required:** `newPassword`, `oldPassword`

### UpdateUserRequest
```json
{
  "fullName": "string",
  "date_of_Birth": "string",
  "email": "string",
  "address": "string",
  "gender": "string",
  "phone": "string",
  "status": "string",
  "identifyNum": "string",
  "roleIds": [0]
}
```
**Required:** `address`, `date_of_Birth`, `email`, `fullName`, `gender`, `identifyNum`, `phone`, `status`

### CreateUserRequest
```json
{
  "fullName": "string",
  "email": "string",
  "phone": "string",
  "identifyNum": "string",
  "address": "string",
  "gender": "string",
  "password": "string",
  "date_of_Birth": "string",
  "roleIds": [0]
}
```
**Required:** `email`, `fullName`, `identifyNum`, `phone`

### FetchUserResponse
```json
{
  "fullName": "string",
  "email": "string",
  "phone": "string",
  "identifyNum": "string",
  "gender": "string",
  "address": "string",
  "date_of_Birth": "string",
  "status": "string",
  "roleIds": [0],
  "updateAt": "2025-08-05T10:30:00Z",
  "updated_by_email": "string"
}
```

### Role
```json
{
  "roleId": 0,
  "name": "string",
  "description": "string",
  "code": "string",
  "created_at": "2025-08-05T10:30:00Z",
  "updated_at": "2025-08-05T10:30:00Z",
  "privileges": [
    {
      "privilegeId": 0,
      "code": "string",
      "description": "string"
    }
  ],
  "createdBy": {
    "userId": 0,
    "fullName": "string",
    "email": "string",
    "identifyNum": "string"
  },
  "updatedBy": {
    "userId": 0,
    "fullName": "string",
    "email": "string",
    "identifyNum": "string"
  }
}
```

### Privilege
```json
{
  "privilegeId": 0,
  "code": "string",
  "description": "string"
}
```

### AuthRequest
```json
{
  "username": "string",
  "password": "string"
}
```

### RegisterRequest
```json
{
  "fullName": "string",
  "email": "string",
  "phone": "string",
  "gender": "string",
  "date_of_Birth": "string",
  "address": "string",
  "password": "string",
  "identifyNum": "string"
}
```

### ResetPasswordRequest
```json
{
  "email": "string",
  "newPassword": "string"
}
```
**Constraints:**
- `email`: Required
- `newPassword`: Required, minimum 8 characters

### VerifyOtpRequest
```json
{
  "email": "string",
  "otp": "string"
}
```

---

## HTTP Status Codes

| Code | Description |
|------|-------------|
| 200  | OK - Request successful |
| 400  | Bad Request - Invalid request parameters |
| 401  | Unauthorized - Authentication required |
| 403  | Forbidden - Insufficient permissions |
| 404  | Not Found - Resource not found |
| 500  | Internal Server Error - Server error |

## Error Response Format

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable error message",
    "details": "Additional error details"
  },
  "timestamp": "2025-08-05T10:30:00Z",
  "path": "/api/endpoint"
}
```

---

**Note:** All timestamps are in ISO 8601 format (UTC). All endpoints require proper authentication via Bearer token unless explicitly stated otherwise.
