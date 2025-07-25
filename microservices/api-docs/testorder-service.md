# API Documentation – *Test Orders Service*

### Base URL

[http://localhost:8082/testorder](http://localhost:8082/testorder)

---

All endpoints require JWT authentication unless stated otherwise.

**Header:**

```http
Authorization: Bearer <your-token>
```

---

# Endpoints
## Placeholder 1
## Placeholder 2

---


### 🧪 Health Check

**`GET /testorder/actuator/health`**

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