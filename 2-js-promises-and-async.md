# JavaScript Promises and Asynchronous Programming

- [JavaScript Promises and Asynchronous Programming](#javascript-promises-and-asynchronous-programming)
    - [**12: Promises in JavaScript**](#12-promises-in-javascript)
      - [**What is a Promise?**](#what-is-a-promise)
      - [**Basic Structure of a Promise**](#basic-structure-of-a-promise)
      - [**Using Promises: A Simple Example**](#using-promises-a-simple-example)
      - [**Chaining Promises**](#chaining-promises)
      - [**Promise Methods**](#promise-methods)
      - [**Error Handling in Promises**](#error-handling-in-promises)
    - [**13: Async/Await in JavaScript**](#13-asyncawait-in-javascript)
      - [**What is Async/Await?**](#what-is-asyncawait)
      - [**Basic Example**](#basic-example)
      - [**Real-World Example**](#real-world-example)
      - [**Using Async/Await with `Promise.all`**](#using-asyncawait-with-promiseall)
      - [**Error Handling in Async/Await**](#error-handling-in-asyncawait)
      - [**Advantages of Async/Await**](#advantages-of-asyncawait)
      - [**Conclusion**](#conclusion)


### **12: Promises in JavaScript**

Promises are a cornerstone of modern JavaScript programming, particularly for managing asynchronous operations. They provide a more readable and structured way to handle asynchronous tasks compared to traditional callbacks, avoiding "callback hell."

---

#### **What is a Promise?**

A promise is an object representing the eventual completion (or failure) of an asynchronous operation and its resulting value. When you create a promise, it is in one of three states:

1. **Pending:** The operation is still in progress.
2. **Fulfilled:** The operation has completed successfully, and a result is available.
3. **Rejected:** The operation has failed, and a reason (error) is available.

#### **Basic Structure of a Promise**

The `Promise` constructor takes a function as an argument, called the executor function. This executor function has two parameters: `resolve` and `reject`. These are callbacks used to indicate success or failure.

```javascript
const promise = new Promise((resolve, reject) => {
    // Perform some asynchronous operation
    let success = true; // Simulate success or failure
    if (success) {
        resolve("Operation succeeded");
    } else {
        reject("Operation failed");
    }
});
```

You handle the result of a promise using the `.then()` method for successful resolution and `.catch()` for errors.

---

#### **Using Promises: A Simple Example**

Let’s simulate a network request to fetch user data:

```javascript
const fetchUserData = new Promise((resolve, reject) => {
    setTimeout(() => {
        let success = true; // Simulate success or failure
        if (success) {
            resolve({ id: 1, name: "Alice" });
        } else {
            reject("Failed to fetch user data");
        }
    }, 2000); // Simulate a delay of 2 seconds
});

fetchUserData
    .then(data => {
        console.log("User Data:", data);
    })
    .catch(error => {
        console.error("Error:", error);
    });
```

**Explanation:**
1. The `fetchUserData` promise simulates an asynchronous task.
2. If successful, the `resolve` callback is called with user data.
3. If there’s an error, the `reject` callback is called with an error message.
4. `.then()` is used to handle the successful result, and `.catch()` handles errors.

---

#### **Chaining Promises**

Promises can be chained to perform sequential asynchronous operations.

Example:
```javascript
function fetchUser(userId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log("Fetched user");
            resolve({ id: userId, name: "Alice" });
        }, 1000);
    });
}

function fetchPosts(userId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log("Fetched posts for user");
            resolve(["Post 1", "Post 2"]);
        }, 1000);
    });
}

function fetchComments(post) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log("Fetched comments for post");
            resolve(["Comment 1", "Comment 2"]);
        }, 1000);
    });
}

fetchUser(1)
    .then(user => fetchPosts(user.id))
    .then(posts => fetchComments(posts[0]))
    .then(comments => {
        console.log("Comments:", comments);
    })
    .catch(error => {
        console.error("Error:", error);
    });
```

**Explanation:**
1. Each `.then()` returns a new promise, allowing you to chain the next asynchronous operation.
2. If any promise in the chain fails, the `.catch()` at the end handles the error.

---

#### **Promise Methods**

**1. `Promise.all()`**
Executes multiple promises in parallel and resolves when all of them succeed, or rejects if any of them fail.

Example:
```javascript
const promise1 = Promise.resolve("Result 1");
const promise2 = Promise.resolve("Result 2");
const promise3 = Promise.resolve("Result 3");

Promise.all([promise1, promise2, promise3])
    .then(results => {
        console.log("All results:", results);
    })
    .catch(error => {
        console.error("Error:", error);
    });
```

**2. `Promise.race()`**
Resolves or rejects as soon as any promise in the array resolves or rejects.

Example:
```javascript
const slow = new Promise(resolve => setTimeout(() => resolve("Slow"), 3000));
const fast = new Promise(resolve => setTimeout(() => resolve("Fast"), 1000));

Promise.race([slow, fast])
    .then(result => {
        console.log("Winner:", result);
    });
```

---

#### **Error Handling in Promises**

Errors in promises are propagated through the chain and can be caught with `.catch()`.

Example:
```javascript
new Promise((resolve, reject) => {
    reject("Something went wrong");
})
    .then(result => {
        console.log(result); // This will not run
    })
    .catch(error => {
        console.error("Caught error:", error);
    });
```

---

### **13: Async/Await in JavaScript**

Async/await is a modern syntax introduced in ES8 that simplifies working with promises. It allows you to write asynchronous code that looks and behaves like synchronous code, making it easier to read and debug.

---

#### **What is Async/Await?**

- `async`: A keyword used to define a function that always returns a promise.
- `await`: A keyword used inside an `async` function to pause execution until a promise resolves.

---

#### **Basic Example**

Here’s a basic example of an `async` function:

```javascript
async function fetchData() {
    const result = await new Promise(resolve => {
        setTimeout(() => resolve("Data fetched"), 2000);
    });
    console.log(result);
}

fetchData();
// Output (after 2 seconds): Data fetched
```

**Explanation:**
1. The `await` keyword pauses the execution of `fetchData` until the promise resolves.
2. Once resolved, the result is assigned to the `result` variable.

---

#### **Real-World Example**

Let’s rewrite the promise chain example using async/await:

```javascript
function fetchUser(userId) {
    return new Promise(resolve => {
        setTimeout(() => resolve({ id: userId, name: "Alice" }), 1000);
    });
}

function fetchPosts(userId) {
    return new Promise(resolve => {
        setTimeout(() => resolve(["Post 1", "Post 2"]), 1000);
    });
}

function fetchComments(post) {
    return new Promise(resolve => {
        setTimeout(() => resolve(["Comment 1", "Comment 2"]), 1000);
    });
}

async function fetchAllData() {
    try {
        const user = await fetchUser(1);
        console.log("User:", user);

        const posts = await fetchPosts(user.id);
        console.log("Posts:", posts);

        const comments = await fetchComments(posts[0]);
        console.log("Comments:", comments);
    } catch (error) {
        console.error("Error:", error);
    }
}

fetchAllData();
// Output:
// User: { id: 1, name: 'Alice' }
// Posts: [ 'Post 1', 'Post 2' ]
// Comments: [ 'Comment 1', 'Comment 2' ]
```

---

#### **Using Async/Await with `Promise.all`**

You can combine async/await with `Promise.all` to run multiple asynchronous tasks in parallel:

```javascript
async function fetchParallelData() {
    const [user, posts] = await Promise.all([
        fetchUser(1),
        fetchPosts(1)
    ]);
    console.log("User:", user);
    console.log("Posts:", posts);
}

fetchParallelData();
// Output:
// User: { id: 1, name: 'Alice' }
// Posts: [ 'Post 1', 'Post 2' ]
```

---

#### **Error Handling in Async/Await**

Use `try...catch` to handle errors in async functions:

```javascript
async function fetchWithError() {
    try {
        const user = await new Promise((_, reject) =>
            setTimeout(() => reject("Failed to fetch user"), 1000)
        );
        console.log(user);
    } catch (error) {
        console.error("Error:", error);
    }
}

fetchWithError();
// Output:
// Error: Failed to fetch user
```

---

#### **Advantages of Async/Await**

1. **Readable Code:** It eliminates the need for chaining `.then()` and `.catch()`, resulting in cleaner and more readable code.
2. **Error Handling:** Errors can be handled with a simple `try...catch` block.
3. **Debugging:** Debugging async/await code is easier since it behaves more like synchronous code.

---

#### **Conclusion**

Async/await simplifies asynchronous programming by providing a cleaner and more synchronous-looking syntax for handling promises. While promises and async/await are both powerful tools for managing asynchronous code, async/await offers a more natural way to express logic, making it an essential feature of modern JavaScript development.

More info about promises can be found at:
- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [JavaScript.info](https://javascript.info/promise-basics)
- [W3Schools](https://www.w3schools.com/js/js_promise.asp)
- [Google Developers](https://web.dev/articles/promises)


More info about async/await can be found at:
- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [JavaScript.info](https://javascript.info/async-await)
- [W3Schools](https://www.w3schools.com/js/js_async.asp)
- [Google Developers](https://web.dev/articles/async-functions)

