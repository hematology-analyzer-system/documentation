# Patient Service API Documentation

**Version:** 1.0.0  
**Base URL:** `http://localhost:8081/patient`  
**Contact:** Healthcare Team (team@healthcare.com)

## Overview

API documentation for Patient microservice providing comprehensive patient record management functionality including CRUD operations, search, and filtering capabilities.

## Authentication

This API uses Bearer Token authentication. Include the following header in your requests:

```
Authorization: Bearer <your_jwt_token>
```

## Table of Contents

- [Patient Management](#patient-management)
- [Data Models](#data-models)
- [HTTP Status Codes](#http-status-codes)
- [Error Response Format](#error-response-format)

---

## Patient Management

### GET /patients
Retrieve all patient records with pagination support.

**Query Parameters:**
- `pageNo` (integer, optional): Page number (default: 0)
- `pageSize` (integer, optional): Number of records per page (default: 10)
- `sortBy` (string, optional): Field to sort by (default: "id")
- `sortDirection` (string, optional): Sort direction - "asc" or "desc" (default: "asc")

**Response:** `200 OK`
```json
{
  "totalElements": 100,
  "totalPages": 10,
  "pageable": {
    "unpaged": false,
    "paged": true,
    "pageSize": 10,
    "pageNumber": 0,
    "offset": 0,
    "sort": {
      "sorted": true,
      "unsorted": false,
      "empty": false
    }
  },
  "numberOfElements": 10,
  "size": 10,
  "content": [
    {
      "id": 1,
      "fullName": "John Doe",
      "address": "123 Main St, City, State",
      "gender": "MALE",
      "dateOfBirth": "1990-05-15",
      "phone": "+1234567890",
      "email": "john.doe@email.com",
      "createdAt": "2025-08-05T10:30:00Z",
      "updatedAt": "2025-08-05T10:30:00Z"
    }
  ],
  "number": 0,
  "sort": {
    "sorted": true,
    "unsorted": false,
    "empty": false
  },
  "first": true,
  "last": false,
  "empty": false
}
```

### POST /patients
Create a new patient record.

**Request Body:**
```json
{
  "fullName": "John Doe",
  "address": "123 Main St, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1234567890",
  "email": "john.doe@email.com"
}
```

**Required Fields:** `address`, `dateOfBirth`, `email`, `fullName`, `gender`, `phone`

**Gender Values:** `MALE`, `FEMALE`, `OTHER`

**Response:** `200 OK`
```json
{
  "id": 1,
  "fullName": "John Doe",
  "address": "123 Main St, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1234567890",
  "email": "john.doe@email.com",
  "createdAt": "2025-08-05T10:30:00Z",
  "updatedAt": "2025-08-05T10:30:00Z"
}
```

### GET /patients/{id}
Retrieve a specific patient record by ID.

**Parameters:**
- `id` (path, required): Patient ID (integer)

**Response:** `200 OK`
```json
{
  "id": 1,
  "fullName": "John Doe",
  "address": "123 Main St, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1234567890",
  "email": "john.doe@email.com",
  "createdAt": "2025-08-05T10:30:00Z",
  "updatedAt": "2025-08-05T10:30:00Z"
}
```

### PUT /patients/{id}
Update an existing patient record.

**Parameters:**
- `id` (path, required): Patient ID (integer)

**Request Body:**
```json
{
  "fullName": "John Doe",
  "address": "456 New Address, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1234567890",
  "email": "john.doe@newemail.com"
}
```

**Gender Values:** `MALE`, `FEMALE`, `OTHER`

**Response:** `200 OK`
```json
{
  "id": 1,
  "fullName": "John Doe",
  "address": "456 New Address, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1234567890",
  "email": "john.doe@newemail.com",
  "createdAt": "2025-08-05T10:30:00Z",
  "updatedAt": "2025-08-05T12:45:00Z"
}
```

### DELETE /patients/{id}
Delete a patient record by ID.

**Parameters:**
- `id` (path, required): Patient ID (integer)

**Response:** `200 OK`
```json
{
  "id": 1,
  "fullName": "John Doe",
  "address": "123 Main St, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1234567890",
  "email": "john.doe@email.com",
  "createdAt": "2025-08-05T10:30:00Z",
  "updatedAt": "2025-08-05T10:30:00Z"
}
```

### GET /patients/filter
Search and filter patient records with advanced options.

**Query Parameters:**
- `searchText` (string, optional): Text to search across patient fields
- `sortBy` (string, optional): Field to sort by (default: "id")
- `direction` (string, optional): Sort direction - "asc" or "desc" (default: "asc")
- `page` (integer, optional): Page number (default: 1)
- `size` (integer, optional): Number of records per page (default: 10)

**Example Request:**
```
GET /patients/filter?searchText=john&sortBy=fullName&direction=asc&page=1&size=10
```

**Response:** `200 OK`
```json
{
  "totalElements": 25,
  "totalPages": 3,
  "pageable": {
    "unpaged": false,
    "paged": true,
    "pageSize": 10,
    "pageNumber": 0,
    "offset": 0,
    "sort": {
      "sorted": true,
      "unsorted": false,
      "empty": false
    }
  },
  "numberOfElements": 10,
  "size": 10,
  "content": [
    {
      "id": 1,
      "fullName": "John Doe",
      "address": "123 Main St, City, State",
      "gender": "MALE",
      "dateOfBirth": "1990-05-15",
      "phone": "+1234567890",
      "email": "john.doe@email.com",
      "createdAt": "2025-08-05T10:30:00Z",
      "updatedAt": "2025-08-05T10:30:00Z"
    },
    {
      "id": 2,
      "fullName": "Jane Smith",
      "address": "456 Oak Ave, City, State",
      "gender": "FEMALE",
      "dateOfBirth": "1985-12-22",
      "phone": "+1987654321",
      "email": "jane.smith@email.com",
      "createdAt": "2025-08-05T11:15:00Z",
      "updatedAt": "2025-08-05T11:15:00Z"
    }
  ],
  "number": 0,
  "sort": {
    "sorted": true,
    "unsorted": false,
    "empty": false
  },
  "first": true,
  "last": false,
  "empty": false
}
```

---

## Data Models

### AddPatientRequest
Request model for creating a new patient record.

```json
{
  "fullName": "string",
  "address": "string",
  "gender": "MALE|FEMALE|OTHER",
  "dateOfBirth": "date (YYYY-MM-DD)",
  "phone": "string",
  "email": "string"
}
```

**Required Fields:** `address`, `dateOfBirth`, `email`, `fullName`, `gender`, `phone`

**Field Constraints:**
- `dateOfBirth`: Must be in YYYY-MM-DD format
- `gender`: Must be one of: `MALE`, `FEMALE`, `OTHER`
- `email`: Must be a valid email format

### UpdatePatientRequest
Request model for updating an existing patient record.

```json
{
  "fullName": "string",
  "address": "string",
  "gender": "MALE|FEMALE|OTHER",
  "dateOfBirth": "date (YYYY-MM-DD)",
  "phone": "string",
  "email": "string"
}
```

**Field Constraints:**
- `dateOfBirth`: Must be in YYYY-MM-DD format
- `gender`: Must be one of: `MALE`, `FEMALE`, `OTHER`
- `email`: Must be a valid email format

### PatientRecordResponse
Response model containing complete patient information.

```json
{
  "id": "integer",
  "fullName": "string",
  "address": "string",
  "gender": "MALE|FEMALE|OTHER",
  "dateOfBirth": "date (YYYY-MM-DD)",
  "phone": "string",
  "email": "string",
  "createdAt": "datetime (ISO 8601)",
  "updatedAt": "datetime (ISO 8601)"
}
```

### PagePatientRecordResponse
Paginated response model for patient record collections.

```json
{
  "totalElements": "integer (int64)",
  "totalPages": "integer (int32)",
  "pageable": {
    "unpaged": "boolean",
    "paged": "boolean",
    "pageSize": "integer (int32)",
    "pageNumber": "integer (int32)",
    "offset": "integer (int64)",
    "sort": {
      "sorted": "boolean",
      "unsorted": "boolean",
      "empty": "boolean"
    }
  },
  "numberOfElements": "integer (int32)",
  "size": "integer (int32)",
  "content": [
    {
      "id": "integer",
      "fullName": "string",
      "address": "string",
      "gender": "MALE|FEMALE|OTHER",
      "dateOfBirth": "date (YYYY-MM-DD)",
      "phone": "string",
      "email": "string",
      "createdAt": "datetime (ISO 8601)",
      "updatedAt": "datetime (ISO 8601)"
    }
  ],
  "number": "integer (int32)",
  "sort": {
    "sorted": "boolean",
    "unsorted": "boolean",
    "empty": "boolean"
  },
  "first": "boolean",
  "last": "boolean",
  "empty": "boolean"
}
```

### PageableObject
Pagination information object.

```json
{
  "unpaged": "boolean",
  "paged": "boolean",
  "pageSize": "integer (int32)",
  "pageNumber": "integer (int32)",
  "offset": "integer (int64)",
  "sort": {
    "sorted": "boolean",
    "unsorted": "boolean",
    "empty": "boolean"
  }
}
```

### SortObject
Sorting information object.

```json
{
  "sorted": "boolean",
  "unsorted": "boolean",
  "empty": "boolean"
}
```

---

## HTTP Status Codes

| Code | Description |
|------|-------------|
| 200  | OK - Request successful |
| 400  | Bad Request - Invalid request parameters or body |
| 401  | Unauthorized - Authentication required |
| 403  | Forbidden - Insufficient permissions |
| 404  | Not Found - Patient record not found |
| 422  | Unprocessable Entity - Invalid data format |
| 500  | Internal Server Error - Server error |

## Error Response Format

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable error message",
    "details": "Additional error details",
    "field": "field_name (for validation errors)"
  },
  "timestamp": "2025-08-05T10:30:00Z",
  "path": "/patients/endpoint"
}
```

## Common Error Scenarios

### Validation Errors
When required fields are missing or invalid:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request data",
    "details": "fullName is required and cannot be empty",
    "field": "fullName"
  },
  "timestamp": "2025-08-05T10:30:00Z",
  "path": "/patients"
}
```

### Patient Not Found
When trying to access a non-existent patient:

```json
{
  "error": {
    "code": "PATIENT_NOT_FOUND",
    "message": "Patient with ID 999 not found",
    "details": "The requested patient record does not exist"
  },
  "timestamp": "2025-08-05T10:30:00Z",
  "path": "/patients/999"
}
```

---

## Usage Examples

### Create a New Patient
```bash
curl -X POST http://localhost:8081/patient/patients \
  -H "Authorization: Bearer your_jwt_token" \
  -H "Content-Type: application/json" \
  -d '{
    "fullName": "Alice Johnson",
    "address": "789 Pine St, Springfield, IL",
    "gender": "FEMALE",
    "dateOfBirth": "1992-03-10",
    "phone": "+1555123456",
    "email": "alice.johnson@email.com"
  }'
```

### Search Patients
```bash
curl -X GET "http://localhost:8081/patient/patients/filter?searchText=alice&sortBy=fullName&direction=asc&page=1&size=10" \
  -H "Authorization: Bearer your_jwt_token"
```

### Update Patient Information
```bash
curl -X PUT http://localhost:8081/patient/patients/1 \
  -H "Authorization: Bearer your_jwt_token" \
  -H "Content-Type: application/json" \
  -d '{
    "fullName": "Alice Johnson-Smith",
    "address": "789 Pine St, Springfield, IL",
    "gender": "FEMALE",
    "dateOfBirth": "1992-03-10",
    "phone": "+1555123456",
    "email": "alice.johnson.smith@email.com"
  }'
```

---

**Note:** All timestamps are in ISO 8601 format (UTC). Date fields use YYYY-MM-DD format. All endpoints require proper authentication via Bearer token.
