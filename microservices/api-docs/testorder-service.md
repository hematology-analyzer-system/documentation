# TestOrder Service API Documentation

**Version:** 1.0.0  
**Base URL:** `http://localhost:8082/testorder`  
**Contact:** Healthcare Team (team@healthcare.com)

## Overview

API documentation for TestOrder microservice providing comprehensive test order management, result handling, and commenting functionality for healthcare laboratory operations.

## Authentication

This API uses Bearer Token authentication. Include the following header in your requests:

```
Authorization: Bearer <your_jwt_token>
```

## Table of Contents

- [Test Order Management](#test-order-management)
- [Result Management](#result-management)
- [Comment Management](#comment-management)
- [Data Models](#data-models)
- [HTTP Status Codes](#http-status-codes)
- [Error Response Format](#error-response-format)

---

## Test Order Management

### POST /testorder/create
Create a new test order without specifying patient ID.

**Request Body:**
```json
{
  "fullName": "John Doe",
  "dateOfBirth": "1990-05-15",
  "gender": "MALE",
  "address": "123 Main St, City, State",
  "phoneNumber": "+1234567890",
  "email": "john.doe@email.com"
}
```

**Required Fields:** `address`, `dateOfBirth`, `email`, `fullName`, `gender`, `phoneNumber`

**Gender Values:** `MALE`, `FEMALE`

**Response:** `200 OK`
```json
{
  "testId": 1,
  "status": "PENDING",
  "updateBy": "admin@healthcare.com",
  "runBy": null,
  "runAt": "2025-08-05T10:30:00Z",
  "results": [],
  "comments": [],
  "fullName": "John Doe",
  "address": "123 Main St, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1234567890"
}
```

### POST /testorder/create/{id}
Create a test order for an existing patient by patient ID.

**Parameters:**
- `id` (path, required): Patient ID (integer)

**Response:** `200 OK`
```json
{
  "testId": 1,
  "status": "PENDING",
  "updateBy": "admin@healthcare.com",
  "runBy": null,
  "runAt": "2025-08-05T10:30:00Z",
  "results": [],
  "comments": [],
  "fullName": "John Doe",
  "address": "123 Main St, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1234567890"
}
```

### GET /testorder/{TOId}
Retrieve detailed information about a specific test order.

**Parameters:**
- `TOId` (path, required): Test Order ID (integer)

**Response:** `200 OK`
```json
{
  "testId": 1,
  "status": "COMPLETED",
  "updateBy": "admin@healthcare.com",
  "runBy": "technician@healthcare.com",
  "runAt": "2025-08-05T10:30:00Z",
  "results": [
    {
      "id": 1,
      "reviewed": true,
      "updateBy": "doctor@healthcare.com",
      "createdAt": "2025-08-05T11:00:00Z",
      "comment_result": [
        {
          "id": 1,
          "content": "Results within normal range",
          "createdBy": "doctor@healthcare.com",
          "updateBy": "doctor@healthcare.com",
          "createdAt": "2025-08-05T11:30:00Z"
        }
      ],
      "detailResults": [
        {
          "paramName": "Glucose",
          "unit": "mg/dL",
          "value": 95.5,
          "rangeMin": 70.0,
          "rangeMax": 100.0
        },
        {
          "paramName": "Cholesterol",
          "unit": "mg/dL",
          "value": 180.0,
          "rangeMin": 125.0,
          "rangeMax": 200.0
        }
      ]
    }
  ],
  "comments": [
    {
      "id": 1,
      "content": "Patient fasted for 12 hours before test",
      "createdBy": "technician@healthcare.com",
      "updateBy": "technician@healthcare.com",
      "createdAt": "2025-08-05T10:45:00Z"
    }
  ],
  "fullName": "John Doe",
  "address": "123 Main St, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1234567890"
}
```

### PUT /testorder/{TOId}
Update test order patient information.

**Parameters:**
- `TOId` (path, required): Test Order ID (integer)

**Request Body:**
```json
{
  "fullName": "John Doe Jr.",
  "address": "456 Oak Ave, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1987654321"
}
```

**Required Fields:** `address`, `dateOfBirth`, `fullName`, `gender`, `phone`

**Response:** `200 OK`
```json
{
  "testId": 1,
  "status": "PENDING",
  "updateBy": "admin@healthcare.com",
  "runBy": null,
  "runAt": "2025-08-05T10:30:00Z",
  "results": [],
  "comments": [],
  "fullName": "John Doe Jr.",
  "address": "456 Oak Ave, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1987654321"
}
```

### DELETE /testorder/{TOId}
Delete a test order.

**Parameters:**
- `TOId` (path, required): Test Order ID (integer)

**Response:** `200 OK`
```json
"Test order deleted successfully"
```

### GET /testorder/search
Search test orders with pagination and sorting.

**Query Parameters:**
- `page` (integer, optional): Page number (default: 0)
- `size` (integer, optional): Page size (default: 5)
- `keyword` (string, optional): Search keyword
- `sortBy` (string, optional): Sort field (default: "runAt")
- `direction` (string, optional): Sort direction (default: "asc")

**Response:** `200 OK`
```json
{
  "list": [
    {
      "testId": 1,
      "status": "COMPLETED",
      "updateBy": "admin@healthcare.com",
      "runBy": "technician@healthcare.com",
      "runAt": "2025-08-05T10:30:00Z",
      "results": [],
      "comments": [],
      "fullName": "John Doe",
      "address": "123 Main St, City, State",
      "gender": "MALE",
      "dateOfBirth": "1990-05-15",
      "phone": "+1234567890"
    }
  ],
  "totalElements": 25,
  "totalPages": 5,
  "currentPage": 0,
  "totalSize": 5,
  "hasNext": true,
  "hasPrevious": false,
  "sortBy": "runAt",
  "sortOrder": "asc",
  "empty": false,
  "message": "Search completed successfully"
}
```

### GET /testorder/filter
Filter test orders with advanced search options including date range.

**Query Parameters:**
- `searchText` (string, optional): Text search across test order fields
- `fromDate` (string, optional): Start date filter (YYYY-MM-DD)
- `toDate` (string, optional): End date filter (YYYY-MM-DD)
- `sortBy` (string, optional): Sort field (default: "runAt")
- `direction` (string, optional): Sort direction (default: "asc")
- `offsetPage` (integer, optional): Page offset (default: 1)
- `limitOnePage` (integer, optional): Records per page (default: 12)

**Response:** `200 OK`
```json
{
  "totalElements": 15,
  "totalPages": 2,
  "pageable": {
    "pageNumber": 0,
    "paged": true,
    "pageSize": 12,
    "unpaged": false,
    "offset": 0,
    "sort": {
      "sorted": true,
      "unsorted": false,
      "empty": false
    }
  },
  "numberOfElements": 12,
  "size": 12,
  "content": [
    {
      "id": 1,
      "fullName": "John Doe",
      "age": 35,
      "gender": "MALE",
      "phone": "+1234567890",
      "status": "COMPLETED",
      "createdBy": "admin@healthcare.com",
      "runBy": "technician@healthcare.com",
      "runAt": "2025-08-05T10:30:00Z"
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

### GET /testorder/grpc/{id}
Test gRPC communication for a specific patient ID.

**Parameters:**
- `id` (path, required): Patient ID (integer)

**Response:** `200 OK`
```json
{
  "testId": 1,
  "status": "PENDING",
  "updateBy": "admin@healthcare.com",
  "runBy": null,
  "runAt": "2025-08-05T10:30:00Z",
  "results": [],
  "comments": [],
  "fullName": "John Doe",
  "address": "123 Main St, City, State",
  "gender": "MALE",
  "dateOfBirth": "1990-05-15",
  "phone": "+1234567890"
}
```

---

## Result Management

### POST /result/gen/{testorderId}
Generate test results for a specific test order.

**Parameters:**
- `testorderId` (path, required): Test Order ID (integer)

**Response:** `200 OK`
```json
{
  "reviewed": false,
  "resultList": [
    {
      "paramName": "Glucose",
      "unit": "mg/dL",
      "value": 95.5,
      "rangeMin": 70.0,
      "rangeMax": 100.0
    },
    {
      "paramName": "Cholesterol",
      "unit": "mg/dL",
      "value": 180.0,
      "rangeMin": 125.0,
      "rangeMax": 200.0
    },
    {
      "paramName": "Triglycerides",
      "unit": "mg/dL",
      "value": 150.0,
      "rangeMin": 50.0,
      "rangeMax": 150.0
    }
  ]
}
```

### PUT /result/{resultId}
Update and review test result details.

**Parameters:**
- `resultId` (path, required): Result ID (integer)

**Request Body:**
```json
[
  {
    "paramName": "Glucose",
    "value": 98.0
  },
  {
    "paramName": "Cholesterol",
    "value": 175.0
  }
]
```

**Response:** `200 OK`
```json
[
  {
    "paramName": "Glucose",
    "unit": "mg/dL",
    "value": 98.0,
    "rangeMin": 70.0,
    "rangeMax": 100.0
  },
  {
    "paramName": "Cholesterol",
    "unit": "mg/dL",
    "value": 175.0,
    "rangeMin": 125.0,
    "rangeMax": 200.0
  }
]
```

---

## Comment Management

### POST /comment/addTO/{id}
Add a comment to a test order.

**Parameters:**
- `id` (path, required): Test Order ID (integer)

**Request Body:**
```json
{
  "content": "Patient reported feeling dizzy during blood draw"
}
```

**Required Fields:** `content`

**Response:** `200 OK`
```json
{
  "content": "Patient reported feeling dizzy during blood draw",
  "createdAt": "2025-08-05T10:45:00Z"
}
```

### POST /comment/addResult/{id}
Add a comment to a test result.

**Parameters:**
- `id` (path, required): Result ID (integer)

**Request Body:**
```json
{
  "content": "Results reviewed and approved by Dr. Smith"
}
```

**Required Fields:** `content`

**Response:** `200 OK`
```json
{
  "content": "Results reviewed and approved by Dr. Smith",
  "createdAt": "2025-08-05T11:30:00Z"
}
```

### PUT /comment/testorder/{commentId}
Update a test order comment.

**Parameters:**
- `commentId` (path, required): Comment ID (integer)

**Request Body:**
```json
{
  "content": "Updated: Patient was stable during blood draw procedure"
}
```

**Required Fields:** `content`

**Response:** `200 OK`
```json
{
  "content": "Updated: Patient was stable during blood draw procedure",
  "createdAt": "2025-08-05T10:45:00Z"
}
```

### PUT /comment/result/{commentId}
Update a result comment.

**Parameters:**
- `commentId` (path, required): Comment ID (integer)

**Request Body:**
```json
{
  "content": "Updated: Results reviewed and verified by Dr. Smith"
}
```

**Required Fields:** `content`

**Response:** `200 OK`
```json
{
  "content": "Updated: Results reviewed and verified by Dr. Smith",
  "createdAt": "2025-08-05T11:30:00Z"
}
```

### DELETE /comment/testorder/{commentId}
Delete a test order comment.

**Parameters:**
- `commentId` (path, required): Comment ID (integer)

**Response:** `200 OK`
```json
"Comment deleted successfully"
```

### DELETE /comment/result/{commentId}
Delete a result comment.

**Parameters:**
- `commentId` (path, required): Comment ID (integer)

**Response:** `200 OK`
```json
"Comment deleted successfully"
```

---

## Data Models

### AddTORequest
Request model for creating a new test order.

```json
{
  "fullName": "string",
  "dateOfBirth": "date (YYYY-MM-DD)",
  "gender": "MALE|FEMALE",
  "address": "string",
  "phoneNumber": "string",
  "email": "string"
}
```

**Required Fields:** `address`, `dateOfBirth`, `email`, `fullName`, `gender`, `phoneNumber`

**Field Constraints:**
- `fullName`: Minimum length 1
- `dateOfBirth`: Must be in YYYY-MM-DD format
- `gender`: Must be either `MALE` or `FEMALE`
- `address`: Minimum length 1
- `phoneNumber`: Minimum length 1
- `email`: Minimum length 1

### UpdateTORequest
Request model for updating test order patient information.

```json
{
  "fullName": "string",
  "address": "string",
  "gender": "MALE|FEMALE",
  "dateOfBirth": "date (YYYY-MM-DD)",
  "phone": "string"
}
```

**Required Fields:** `address`, `dateOfBirth`, `fullName`, `gender`, `phone`

### TOResponse
Complete test order response model.

```json
{
  "testId": "integer (int64)",
  "status": "string",
  "updateBy": "string",
  "runBy": "string",
  "runAt": "datetime (ISO 8601)",
  "results": [
    {
      "id": "integer (int64)",
      "reviewed": "boolean",
      "updateBy": "string",
      "createdAt": "datetime (ISO 8601)",
      "comment_result": [
        {
          "id": "integer (int64)",
          "content": "string",
          "createdBy": "string",
          "updateBy": "string",
          "createdAt": "datetime (ISO 8601)"
        }
      ],
      "detailResults": [
        {
          "paramName": "string",
          "unit": "string",
          "value": "number (double)",
          "rangeMin": "number (double)",
          "rangeMax": "number (double)"
        }
      ]
    }
  ],
  "comments": [
    {
      "id": "integer (int64)",
      "content": "string",
      "createdBy": "string",
      "updateBy": "string",
      "createdAt": "datetime (ISO 8601)"
    }
  ],
  "fullName": "string",
  "address": "string",
  "gender": "MALE|FEMALE",
  "dateOfBirth": "date (YYYY-MM-DD)",
  "phone": "string"
}
```

### ResultResponse
Test result response model.

```json
{
  "reviewed": "boolean",
  "resultList": [
    {
      "paramName": "string",
      "unit": "string",
      "value": "number (double)",
      "rangeMin": "number (double)",
      "rangeMax": "number (double)"
    }
  ]
}
```

### ReviewResultRequest
Request model for reviewing and updating result values.

```json
{
  "paramName": "string",
  "value": "number (double)"
}
```

### AddCommentRequest
Request model for adding comments.

```json
{
  "content": "string"
}
```

**Required Fields:** `content`

**Field Constraints:**
- `content`: Minimum length 1

### UpdateCommentRequest
Request model for updating comments.

```json
{
  "content": "string"
}
```

**Required Fields:** `content`

**Field Constraints:**
- `content`: Minimum length 1

### CommentResponse
Comment response model.

```json
{
  "content": "string",
  "createdAt": "datetime (ISO 8601)"
}
```

### DetailResultResponse
Detailed test result parameter model.

```json
{
  "paramName": "string",
  "unit": "string",
  "value": "number (double)",
  "rangeMin": "number (double)",
  "rangeMax": "number (double)"
}
```

### PageTOResponse
Paginated test order response model.

```json
{
  "list": [
    {
      "testId": "integer (int64)",
      "status": "string",
      "updateBy": "string",
      "runBy": "string",
      "runAt": "datetime (ISO 8601)",
      "results": [],
      "comments": [],
      "fullName": "string",
      "address": "string",
      "gender": "MALE|FEMALE",
      "dateOfBirth": "date (YYYY-MM-DD)",
      "phone": "string"
    }
  ],
  "totalElements": "integer (int64)",
  "totalPages": "integer (int32)",
  "currentPage": "integer (int32)",
  "totalSize": "integer (int32)",
  "hasNext": "boolean",
  "hasPrevious": "boolean",
  "sortBy": "string",
  "sortOrder": "string",
  "empty": "boolean",
  "message": "string"
}
```

### SearchDTO
Search result item model.

```json
{
  "id": "integer (int64)",
  "fullName": "string",
  "age": "integer (int32)",
  "gender": "MALE|FEMALE",
  "phone": "string",
  "status": "string",
  "createdBy": "string",
  "runBy": "string",
  "runAt": "datetime (ISO 8601)"
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
| 404  | Not Found - Resource not found |
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
  "path": "/testorder/endpoint"
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
  "path": "/testorder/create"
}
```

### Test Order Not Found
When trying to access a non-existent test order:

```json
{
  "error": {
    "code": "TEST_ORDER_NOT_FOUND",
    "message": "Test order with ID 999 not found",
    "details": "The requested test order does not exist"
  },
  "timestamp": "2025-08-05T10:30:00Z",
  "path": "/testorder/999"
}
```

### Result Not Found
When trying to access a non-existent result:

```json
{
  "error": {
    "code": "RESULT_NOT_FOUND",
    "message": "Result with ID 999 not found",
    "details": "The requested test result does not exist"
  },
  "timestamp": "2025-08-05T10:30:00Z",
  "path": "/result/999"
}
```

---

## Usage Examples

### Create Test Order for New Patient
```bash
curl -X POST http://localhost:8082/testorder/testorder/create \
  -H "Authorization: Bearer your_jwt_token" \
  -H "Content-Type: application/json" \
  -d '{
    "fullName": "Alice Johnson",
    "dateOfBirth": "1985-12-22",
    "gender": "FEMALE",
    "address": "789 Pine St, Springfield, IL",
    "phoneNumber": "+1555123456",
    "email": "alice.johnson@email.com"
  }'
```

### Generate Test Results
```bash
curl -X POST http://localhost:8082/testorder/result/gen/1 \
  -H "Authorization: Bearer your_jwt_token"
```

### Search Test Orders
```bash
curl -X GET "http://localhost:8082/testorder/testorder/search?keyword=alice&sortBy=runAt&direction=desc&page=0&size=10" \
  -H "Authorization: Bearer your_jwt_token"
```

### Add Comment to Test Order
```bash
curl -X POST http://localhost:8082/testorder/comment/addTO/1 \
  -H "Authorization: Bearer your_jwt_token" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Patient reported no symptoms during collection"
  }'
```

### Update Test Results
```bash
curl -X PUT http://localhost:8082/testorder/result/1 \
  -H "Authorization: Bearer your_jwt_token" \
  -H "Content-Type: application/json" \
  -d '[
    {
      "paramName": "Glucose",
      "value": 95.5
    },
    {
      "paramName": "Cholesterol",
      "value": 180.0
    }
  ]'
```

---

**Note:** All timestamps are in ISO 8601 format (UTC). Date fields use YYYY-MM-DD format. All endpoints require proper authentication via Bearer token. The service supports comprehensive test order lifecycle management from creation to result review and commenting.
