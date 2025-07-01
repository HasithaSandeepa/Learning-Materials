# ⚡ Thunks – Understanding and Usage

## 📌 What is a Thunk?

A **thunk** is a function that wraps an expression to delay its evaluation. In JavaScript and especially in Redux, thunks are used to handle **asynchronous logic** by returning a function instead of an action object.

---

## 🔍 Why Use Thunks?

- To perform asynchronous operations like API calls.
- To dispatch multiple actions over time.
- To handle side effects in Redux or similar state management systems.
- Keep action creators pure and simple.

---

## ⚙️ How Thunks Work (Basic Concept)

A thunk is a function that returns another function. In Redux middleware (like `redux-thunk`), when you dispatch a thunk, it gets executed with `dispatch` and `getState` as arguments.

```js
const thunk = () => {
  return (dispatch, getState) => {
    // perform async work here
  };
};
```

---

## 🧩 Example: Async API Call with Thunk

### Setup

```js
// Action Types
const FETCH_DATA_REQUEST = "FETCH_DATA_REQUEST";
const FETCH_DATA_SUCCESS = "FETCH_DATA_SUCCESS";
const FETCH_DATA_FAILURE = "FETCH_DATA_FAILURE";

// Action Creators
const fetchDataRequest = () => ({ type: FETCH_DATA_REQUEST });
const fetchDataSuccess = (data) => ({
  type: FETCH_DATA_SUCCESS,
  payload: data,
});
const fetchDataFailure = (error) => ({
  type: FETCH_DATA_FAILURE,
  payload: error,
});
```

### Thunk Action Creator

```js
const fetchData = () => {
  return (dispatch) => {
    dispatch(fetchDataRequest());

    fetch("https://api.example.com/data")
      .then((response) => response.json())
      .then((data) => dispatch(fetchDataSuccess(data)))
      .catch((error) => dispatch(fetchDataFailure(error.message)));
  };
};
```

---

## 🔄 Dispatching a Thunk

```js
store.dispatch(fetchData());
```

---

## 📚 Using Redux Thunk Middleware

Install the middleware:

```bash
npm install redux-thunk
```

Apply it in your Redux store:

```js
import { createStore, applyMiddleware } from "redux";
import thunk from "redux-thunk";
import rootReducer from "./reducers";

const store = createStore(rootReducer, applyMiddleware(thunk));
```

---

## 📝 Benefits of Thunks

- Simplifies asynchronous dispatching.
- Keeps side effects outside reducers.
- Allows conditional dispatching based on current state.
- Enables complex workflows like chaining or batching actions.

---

## ⚠️ Notes

- Thunks add an extra layer of abstraction which can complicate debugging.
- Alternatives include sagas, observables, or other middleware for complex async flows.

---

## 📝 Author

**👤 Hasitha Sandeepa**

- 🔗 [GitHub](https://github.com/HasithaSandeepa)
- 🔗 [LinkedIn](https://www.linkedin.com/in/hasitha-sandeepa/)
