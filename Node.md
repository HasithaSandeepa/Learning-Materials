# 🌱 Core Concepts of Node.js

## 📌 What is Node.js?

**Node.js** is a JavaScript runtime environment built on Chrome's V8 JavaScript engine. It allows you to run JavaScript code on the server side.

> It uses an event-driven, non-blocking I/O model, making it lightweight and efficient for data-intensive applications.

---

## 🧠 Key Features

- **Single-threaded** but highly scalable
- **Asynchronous and non-blocking**
- Uses **V8 Engine** for high performance
- Built-in support for **modules and file system operations**
- Ideal for **API services**, **real-time applications**, and **microservices**

---

## ⚙️ How Node.js Works

### Architecture

- **Single-threaded event loop**
- **Non-blocking I/O**
- **Callback queue**
- **Worker threads in the background** (libuv library)

### Simplified Flow:

1. Client sends a request.
2. Node adds it to the event queue.
3. The event loop checks if the thread is free.
4. If the task is blocking (e.g., file read), it delegates to worker threads.
5. Once complete, the callback is pushed to the queue.

---

## 🔄 Event Loop

Node.js uses the **event loop** to manage asynchronous operations without blocking the main thread.

```js
console.log("Start");

setTimeout(() => {
  console.log("Inside timeout");
}, 2000);

console.log("End");
```

**Output:**

```
Start
End
Inside timeout
```

---

## 🧩 Modules in Node.js

Modules are reusable blocks of code. Node.js has:

### 1. Built-in Modules

- `fs` (File System)
- `http`
- `path`
- `os`
- `events`

### 2. Custom Modules

Create `math.js`:

```js
function add(a, b) {
  return a + b;
}
module.exports = { add };
```

Use in `index.js`:

```js
const math = require("./math");
console.log(math.add(5, 3));
```

---

## 🧪 Common Built-in Modules

### `fs` – File System

```js
const fs = require("fs");

fs.readFile("demo.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

### `http` – Creating a Server

```js
const http = require("http");

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain" });
  res.end("Hello Hasitha!");
});

server.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

---

## 🛑 Blocking vs Non-Blocking

### Blocking (Synchronous)

```js
const data = fs.readFileSync("demo.txt", "utf8");
console.log(data);
```

### Non-Blocking (Asynchronous)

```js
fs.readFile("demo.txt", "utf8", (err, data) => {
  console.log(data);
});
```

---

## 📦 NPM (Node Package Manager)

Used to manage external libraries and modules.

### Initialize Project

```bash
npm init -y
```

### Install a Package

```bash
npm install axios
```

### Use a Package

```js
const axios = require("axios");

axios.get("https://api.example.com").then((response) => {
  console.log(response.data);
});
```

---

## 🛠️ Global Objects

| Object       | Description                          |
| ------------ | ------------------------------------ |
| `__dirname`  | Directory name of the current module |
| `__filename` | File name of the current module      |
| `require`    | Used to import modules               |
| `module`     | Information about current module     |

---

## 📚 Summary

- Node.js is single-threaded but highly scalable via event loop and callbacks.
- Core modules allow access to system-level operations.
- Everything in Node is modular and reusable.
- Asynchronous programming helps prevent server blocking.
- NPM enables access to a vast ecosystem of libraries.

---

## 📝 Author

**👤 Hasitha Sandeepa**

- 🔗 [GitHub](https://github.com/HasithaSandeepa)
- 🔗 [LinkedIn](https://www.linkedin.com/in/hasitha-sandeepa/)
