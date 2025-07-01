# 📘 MongoDB Aggregate Functions - Complete Guide

## 📌 Introduction

Aggregation operations process data records and return computed results. They are used to group values from multiple documents, perform operations on grouped data, and transform the shape of documents.

In MongoDB, the **`aggregate()`** method is used along with a pipeline of stages to perform data aggregation.

---

## 🏗️ Basic Structure

```js
db.collection.aggregate([
  { stage1 },
  { stage2 },
  ...
])
```

Each stage is an object with a key representing the aggregation operator (e.g., `$match`, `$group`, `$project`) and the value defining its behavior.

---

## 🧱 Common Aggregation Stages

### 1. `$match`

Filters the documents similar to a `find()` query.

```js
db.orders.aggregate([{ $match: { status: "pending" } }]);
```

### 2. `$group`

Groups documents by a specified key and allows accumulation operations (e.g., `$sum`, `$avg`, `$max`).

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customerId",
      totalAmount: { $sum: "$amount" },
    },
  },
]);
```

### 3. `$project`

Shapes the documents by including, excluding, or computing new fields.

```js
db.orders.aggregate([
  {
    $project: {
      item: 1,
      amountWithTax: { $multiply: ["$amount", 1.1] },
      _id: 0,
    },
  },
]);
```

### 4. `$sort`

Sorts the documents.

```js
db.orders.aggregate([{ $sort: { amount: -1 } }]);
```

### 5. `$limit` and `$skip`

Limits the number of documents or skips a certain number.

```js
db.orders.aggregate([{ $sort: { amount: -1 } }, { $skip: 5 }, { $limit: 10 }]);
```

---

## 🧮 Aggregation Operators (Used inside `$group`, `$project`, etc.)

| Operator    | Description                                |
| ----------- | ------------------------------------------ |
| `$sum`      | Sum of numeric values                      |
| `$avg`      | Average of numeric values                  |
| `$min`      | Minimum value                              |
| `$max`      | Maximum value                              |
| `$first`    | First value in group                       |
| `$last`     | Last value in group                        |
| `$push`     | Appends value to an array                  |
| `$addToSet` | Adds unique value to a set (no duplicates) |

---

## 🧪 Example: Aggregate Order Data

**Requirement**: For each customer, get the number of orders and total amount spent.

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customerId",
      orderCount: { $sum: 1 },
      totalAmount: { $sum: "$amount" },
    },
  },
]);
```

---

## 🛠️ Example: Filtering, Grouping, and Sorting

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  {
    $group: {
      _id: "$customerId",
      total: { $sum: "$amount" },
    },
  },
  { $sort: { total: -1 } },
]);
```

---

## 📎 Additional Stages

### `$unwind`

Deconstructs an array field from the input documents to output a document for each element.

```js
db.orders.aggregate([{ $unwind: "$items" }]);
```

### `$lookup`

Performs a left outer join to another collection.

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customerDetails",
    },
  },
]);
```

### `$addFields`

Adds new fields to documents.

```js
db.orders.aggregate([
  {
    $addFields: {
      amountWithTax: { $multiply: ["$amount", 1.15] },
    },
  },
]);
```

---

## 🧾 Practical Case Study

**Collections:**

```js
// orders
{
  _id: 1,
  customerId: 101,
  amount: 500,
  status: "completed",
  date: ISODate("2025-06-01")
}

// customers
{
  _id: 101,
  name: "Hasitha",
  city: "Matara"
}
```

**Task**: Get customer name and total completed order amount.

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  {
    $group: {
      _id: "$customerId",
      totalSpent: { $sum: "$amount" },
    },
  },
  {
    $lookup: {
      from: "customers",
      localField: "_id",
      foreignField: "_id",
      as: "customer",
    },
  },
  { $unwind: "$customer" },
  {
    $project: {
      _id: 0,
      customerName: "$customer.name",
      totalSpent: 1,
    },
  },
]);
```

---

## 🧠 Summary

MongoDB Aggregation Framework is powerful for data analysis and reporting. The key is understanding each stage and operator and how to chain them effectively.

- Use `$match` early to reduce data.
- `$group` for summarizing.
- `$project` to format output.
- `$lookup` for joins.
- Combine multiple stages for advanced processing.

---

## 📝 Author

**👤 Hasitha Sandeepa**

- 🔗 [GitHub](https://github.com/HasithaSandeepa)
- 🔗 [LinkedIn](https://www.linkedin.com/in/hasitha-sandeepa/)
