# 🌐 API (Application Programming Interface) - Complete Guide

## 📌 What is an API?

An **API (Application Programming Interface)** is a set of rules that allows different software components to communicate with each other. APIs define how requests and responses should be structured, enabling systems to share data and functionality.

---

## 📂 Types of APIs

| Type           | Description                                          |
| -------------- | ---------------------------------------------------- |
| **Web APIs**   | Accessed over the internet using HTTP/HTTPS.         |
| **REST APIs**  | Follows REST principles using standard HTTP methods. |
| **SOAP APIs**  | Uses XML and follows strict protocol standards.      |
| **GraphQL**    | Allows clients to request only the data they need.   |
| **Local APIs** | Used within a local system or application.           |

---

## 🌐 What is a REST API?

**REST (Representational State Transfer)** is an architectural style for designing networked applications. A REST API uses standard HTTP methods to interact with resources.

---

## 🔧 HTTP Methods

| Method   | Description                    | Example Use                 |
| -------- | ------------------------------ | --------------------------- |
| `GET`    | Retrieve data from the server  | Get all users               |
| `POST`   | Send new data to the server    | Create a new user           |
| `PUT`    | Update existing data           | Update user info completely |
| `PATCH`  | Partially update existing data | Update only user's email    |
| `DELETE` | Delete existing data           | Delete a user               |

### 🛠️ Example:

```http
GET /users
POST /users
PUT /users/101
DELETE /users/101
```

---

## 📦 JSON Structure Example

### Request:

```http
POST /users
Content-Type: application/json

{
  "name": "Hasitha",
  "email": "hasitha@example.com",
  "role": "admin"
}
```

### Response:

```json
{
  "status": "success",
  "data": {
    "id": 101,
    "name": "Hasitha",
    "email": "hasitha@example.com",
    "role": "admin"
  }
}
```

---

## 📊 HTTP Status Codes

| Code | Meaning               | Description                                      |
| ---- | --------------------- | ------------------------------------------------ |
| 200  | OK                    | Request successful                               |
| 201  | Created               | Resource created successfully                    |
| 204  | No Content            | Request successful but no content to return      |
| 400  | Bad Request           | Client error - malformed syntax or invalid input |
| 401  | Unauthorized          | Authentication is required                       |
| 403  | Forbidden             | Authenticated but not allowed                    |
| 404  | Not Found             | Resource not found                               |
| 409  | Conflict              | Resource already exists                          |
| 422  | Unprocessable Entity  | Valid data structure, but semantically incorrect |
| 500  | Internal Server Error | Server encountered an unexpected condition       |

---

## 🚫 Error Handling in APIs

### Common Error Response Format:

```json
{
  "status": "error",
  "message": "Invalid email format",
  "code": 400
}
```

### Best Practices:

- Always include clear error messages.
- Never expose system errors or database details to clients.
- Log internal errors internally for developers.
- Show friendly and generic messages to users.

---

## 🛡️ Securing Error Messages (Avoid System Leaks)

System errors may contain stack traces, file paths, or database details. These can be dangerous if exposed to the public.

### ✅ Use Custom Error Handling Middleware (Node.js + Express)

```js
// errorHandler.js
module.exports = (err, req, res, next) => {
  console.error(err.stack); // log full error internally (in logs)

  res.status(err.statusCode || 500).json({
    status: "error",
    message: err.isOperational ? err.message : "Something went wrong!",
  });
};
```

### ✅ Mark Errors as Operational

```js
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true;
  }
}
```

### ✅ Use in Routes

```js
app.get("/api/users/:id", (req, res, next) => {
  try {
    const user = users.find((u) => u.id === parseInt(req.params.id));
    if (!user) {
      return next(new AppError("User not found", 404));
    }
    res.status(200).json({ status: "success", data: user });
  } catch (err) {
    next(err); // Forward to error handler
  }
});
```

### ✅ Register Middleware at End of App

```js
const errorHandler = require("./middlewares/errorHandler");
app.use(errorHandler);
```

---

## 💡 Example REST API Endpoints (User Management)

| Endpoint         | Method | Description            |
| ---------------- | ------ | ---------------------- |
| `/api/users`     | GET    | Retrieve all users     |
| `/api/users/:id` | GET    | Retrieve a single user |
| `/api/users`     | POST   | Create a new user      |
| `/api/users/:id` | PUT    | Update a user          |
| `/api/users/:id` | DELETE | Delete a user          |

---

## 🔐 Authentication in APIs

Most APIs are secured using tokens or API keys.

### Common Methods:

- **API Key**
- **JWT (JSON Web Token)**
- **OAuth 2.0**

---

## 📋 Documentation Tips

A good API should be well documented. Use tools like:

- [Swagger / OpenAPI](https://swagger.io/)
- Postman for testing
- GitHub README for simple endpoints

---

## ✅ Summary

- APIs enable communication between software systems.
- REST APIs are widely used with HTTP methods.
- Always handle errors and return meaningful status codes.
- Avoid exposing internal error details for security.
- Use authentication to protect endpoints.
- Document your API clearly for better usability.

---

## 📝 Author

**👤 Hasitha Sandeepa**

- 🔗 [GitHub](https://github.com/HasithaSandeepa)
- 🔗 [LinkedIn](https://www.linkedin.com/in/hasitha-sandeepa/)
