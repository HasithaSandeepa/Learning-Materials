# ⚙️ Core Concepts of Express.js

## 📌 What is Express.js?

**Express.js** is a fast, unopinionated, and minimalist web framework for Node.js. It simplifies the process of building web applications and APIs using JavaScript on the backend.

> Express handles HTTP requests, routes, middleware, and responses, making backend development more organized and efficient.

---

## 🚀 Why Use Express?

- Simplifies routing and server logic
- Built on top of Node.js
- Supports middleware for extensibility
- Enables REST API development with ease
- Widely used in full-stack and microservices architectures

---

## 🏗️ Express Application Structure

```plaintext
myapp/
│
├── node_modules/
├── routes/
│   └── users.js
├── middlewares/
│   └── logger.js
├── index.js
├── package.json
```

---

## 📍 Core Concepts

### 1. Creating an Express App

```js
const express = require("express");
const app = express();
const port = 3000;

app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
```

---

### 2. Routes

A route defines the endpoint (path) and method (GET, POST, etc.) used to handle requests.

```js
app.get("/", (req, res) => {
  res.send("Welcome to Express!");
});

app.post("/users", (req, res) => {
  res.send("User created!");
});
```

#### Route Parameters

```js
app.get("/user/:id", (req, res) => {
  res.send(`User ID: ${req.params.id}`);
});
```

---

### 3. Middleware

Middleware functions are functions that run during the lifecycle of a request to the server.

```js
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next(); // Proceed to next middleware or route
});
```

Types of middleware:

- **Application-level**
- **Router-level**
- **Built-in (`express.json()`, `express.static()`)**
- **Error-handling middleware**

---

### 4. Request and Response Objects

**req** – represents the HTTP request
**res** – represents the HTTP response

```js
app.get("/hello", (req, res) => {
  res.status(200).json({
    message: "Hello Hasitha!",
  });
});
```

---

### 5. Express Router

Router helps you modularize and organize routes.

**routes/users.js:**

```js
const express = require("express");
const router = express.Router();

router.get("/", (req, res) => res.send("All Users"));
router.get("/:id", (req, res) => res.send(`User ${req.params.id}`));

module.exports = router;
```

**index.js:**

```js
const users = require("./routes/users");
app.use("/users", users);
```

---

### 6. Serving Static Files

```js
app.use(express.static("public"));
```

Files in the `public` folder (like HTML, CSS, JS) will be served directly.

---

### 7. JSON Body Parsing

To handle JSON request bodies:

```js
app.use(express.json());
```

Then you can use:

```js
app.post("/api/data", (req, res) => {
  console.log(req.body);
  res.send("Data received");
});
```

---

### 8. Error Handling

Custom error handler middleware:

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: "Something went wrong" });
});
```

Use `next(error)` in routes to trigger it.

---

### 9. HTTP Methods Overview

| Method   | Description               |
| -------- | ------------------------- |
| `GET`    | Retrieve resource         |
| `POST`   | Create resource           |
| `PUT`    | Replace existing resource |
| `PATCH`  | Partially update resource |
| `DELETE` | Remove resource           |

---

### 10. Route Grouping and Organization

Group related routes using routers. Example:

```js
app.use("/api/users", require("./routes/users"));
app.use("/api/products", require("./routes/products"));
```

This results in more maintainable and scalable applications.

---

## 📦 Commonly Used Middleware

| Middleware       | Purpose                              |
| ---------------- | ------------------------------------ |
| `express.json()` | Parse incoming JSON requests         |
| `morgan`         | Log HTTP requests                    |
| `helmet`         | Set secure HTTP headers              |
| `cors`           | Enable Cross-Origin Resource Sharing |
| `dotenv`         | Load environment variables           |

---

## ✅ Summary

- Express is a lightweight web framework for Node.js.
- It simplifies HTTP server and API development.
- Core concepts include routing, middleware, request/response handling, and modular routers.
- Middleware enhances functionality (auth, logging, validation).
- Express is ideal for building scalable and maintainable backend applications.

---

## 📝 Author

**👤 Hasitha Sandeepa**

- 🔗 [GitHub](https://github.com/HasithaSandeepa)
- 🔗 [LinkedIn](https://www.linkedin.com/in/hasitha-sandeepa/)
