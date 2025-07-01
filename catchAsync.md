# 🔄 catchAsync Utility in Node.js / Express

## 📌 What is `catchAsync`?

`catchAsync` is a helper function used to simplify error handling in asynchronous Express route handlers or middleware.

### Why use it?

- Avoid repetitive `try-catch` blocks in every async function.
- Automatically pass errors to Express's error handling middleware via `next()`.
- Makes code cleaner, more readable, and easier to maintain.

---

## ⚙️ How does it work?

Express does not catch rejected promises or async errors automatically. If an async route throws an error, it must be passed to the next middleware using `next(error)`.

`catchAsync` wraps an async function and catches errors, forwarding them automatically.

---

## 🧩 Basic Implementation

```js
const catchAsync = (fn) => {
  return (req, res, next) => {
    fn(req, res, next).catch(next);
  };
};

module.exports = catchAsync;
```

---

## 🔎 Usage Example

### Without `catchAsync` (manual try-catch)

```js
app.get("/users/:id", async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) {
      return res.status(404).json({ message: "User not found" });
    }
    res.json(user);
  } catch (err) {
    next(err);
  }
});
```

### With `catchAsync` (cleaner)

```js
const catchAsync = require("./utils/catchAsync");

app.get(
  "/users/:id",
  catchAsync(async (req, res, next) => {
    const user = await User.findById(req.params.id);
    if (!user) {
      return res.status(404).json({ message: "User not found" });
    }
    res.json(user);
  })
);
```

---

## 📚 Benefits

- Reduces boilerplate code.
- Ensures all async errors are forwarded correctly.
- Improves readability and maintainability.

---

## ⚠️ Important Notes

- Works only with async functions returning promises.
- Requires a global error handling middleware in Express to catch errors passed with `next()`.

Example error middleware:

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ message: "Internal Server Error" });
});
```

---

## 📝 Author

**👤 Hasitha Sandeepa**

- 🔗 [GitHub](https://github.com/HasithaSandeepa)
- 🔗 [LinkedIn](https://www.linkedin.com/in/hasitha-sandeepa/)
