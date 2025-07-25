# API Documentation – *IAM Service*

### Base URL

[http://localhost:8080/iam](http://localhost:8080/iam)

---

All endpoints require JWT authentication unless stated otherwise.

**Header:**

```http
Authorization: Bearer <your-token>
```

---

# Endpoints

## User Management



---


## Role Management



---


## Privilege Management



---


## Authentication Management



---


### 🧪 Health Check

**`GET /iam/actuator/health`**

**Response:**

```json
{
  "status": "UP"
}
```

---

## 📦 Error Codes

| Code | Meaning               |
| ---- | --------------------- |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 500  | Internal Server Error |