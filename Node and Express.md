# 🚀 Node.js & Express.js – Complete Guide

## 📌 Introduction

**Node.js** is a JavaScript runtime built on Chrome’s V8 engine that allows developers to run JavaScript on the server-side.

**Express.js** is a minimal and flexible Node.js web application framework that provides robust features for building web and RESTful APIs.

---

## 🛠️ Why Use Node.js?

- Asynchronous and non-blocking
- Fast and efficient
- Full-stack JavaScript (frontend + backend)
- Rich package ecosystem via **npm**

---

## ⚙️ Setting Up a Node + Express Project

### 1. Initialize a new project

```bash
mkdir myapp
cd myapp
npm init -y
```

### 2. Install Express

```bash
npm install express
```

### 3. Create `index.js`

```js
const express = require("express");
const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("Hello, Hasitha! Welcome to Express.");
});

app.listen(port, () => {
  console.log(`Server is running on http://localhost:${port}`);
});
```

---

## 📦 Express Project Structure

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

## 🧩 Core Concepts of Express

### 1. **Routing**

#### Basic Route

```js
app.get("/about", (req, res) => {
  res.send("This is the about page.");
});
```

#### Route with URL parameter

```js
app.get("/user/:id", (req, res) => {
  res.send(`User ID: ${req.params.id}`);
});
```

### 2. **Middleware**

Middleware functions are functions that have access to the request and response objects. They are used for logging, authentication, etc.

```js
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
});
```

### 3. **Serving JSON**

```js
app.get("/api/data", (req, res) => {
  res.json({ name: "Hasitha", role: "Student" });
});
```

---

## 🧰 Using Express Router

### Create a file `routes/users.js`

```js
const express = require("express");
const router = express.Router();

router.get("/", (req, res) => {
  res.send("All users");
});

router.get("/:id", (req, res) => {
  res.send(`User ID: ${req.params.id}`);
});

module.exports = router;
```

### Use in main file `index.js`

```js
const userRoutes = require("./routes/users");
app.use("/users", userRoutes);
```

---

## 🛡️ Error Handling

### Custom Error Handler Middleware

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ message: "Something went wrong!" });
});
```

### Example of Using Error in Route

```js
app.get("/fail", (req, res, next) => {
  next(new Error("This route failed!"));
});
```

---

## 🔐 Express and Middleware for Security

Install Helmet to secure HTTP headers:

```bash
npm install helmet
```

```js
const helmet = require("helmet");
app.use(helmet());
```

---

## 🧪 Example: Simple CRUD with Express

```js
const express = require("express");
const app = express();
app.use(express.json());

let users = [];

app.post("/users", (req, res) => {
  const user = req.body;
  users.push(user);
  res.status(201).json(user);
});

app.get("/users", (req, res) => {
  res.json(users);
});

app.put("/users/:id", (req, res) => {
  const id = req.params.id;
  users[id] = req.body;
  res.json(users[id]);
});

app.delete("/users/:id", (req, res) => {
  const id = req.params.id;
  users.splice(id, 1);
  res.status(204).send();
});
```

---

## 🔄 Environment Variables

Use `.env` to store secrets:

```bash
npm install dotenv
```

**.env**

```
PORT=4000
```

**index.js**

```js
require("dotenv").config();
const port = process.env.PORT || 3000;
```

---

## 🔍 Useful Packages

| Package   | Purpose                        |
| --------- | ------------------------------ |
| `nodemon` | Auto-restart server on changes |
| `dotenv`  | Load environment variables     |
| `helmet`  | Security headers               |
| `morgan`  | HTTP request logger            |
| `cors`    | Enable CORS                    |

---

## ✅ Summary

- **Node.js** enables server-side JavaScript execution.
- **Express.js** simplifies HTTP routing and middleware handling.
- Use routers and middlewares for modular and maintainable code.
- Always handle errors gracefully and avoid exposing internal errors.
- Use `.env` files for configuration.

---

## 📝 Author

**👤 Hasitha Sandeepa**

- 🔗 [GitHub](https://github.com/HasithaSandeepa)
- 🔗 [LinkedIn](https://www.linkedin.com/in/hasitha-sandeepa/)
