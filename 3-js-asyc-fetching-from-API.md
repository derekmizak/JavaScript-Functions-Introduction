### **Chapter 14: Async/Await with Fetching Data from Public APIs**

Async/await becomes especially powerful when dealing with real-world asynchronous operations such as fetching data from APIs. In this chapter, we will demonstrate how to use async/await to make HTTP requests to publicly available APIs, handle responses, and process data.

---

#### **What is an API?**

An API (Application Programming Interface) allows applications to communicate with each other. For example, a weather API provides weather data that you can use in your application. To fetch data from an API in JavaScript, you can use the `fetch()` function, which returns a promise.

---

#### **Using Fetch with Async/Await**

The `fetch()` function makes HTTP requests and returns a promise. Using `async/await`, we can simplify the process of handling these promises.

Here’s a simple example of fetching data from a public API (JSONPlaceholder, a free API for testing and prototyping).

---

**Basic Example: Fetching Data**

```javascript
async function fetchUserData() {
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        const users = await response.json(); // Parse JSON from the response
        console.log(users);
    } catch (error) {
        console.error("Error fetching user data:", error);
    }
}

fetchUserData();
```

**Explanation:**
1. **`await fetch(url)`**: Fetches data from the given URL. The `await` keyword ensures the code waits until the data is fetched.
2. **Check Response Status**: The `response.ok` property indicates if the HTTP request was successful.
3. **Parse JSON**: Use `response.json()` to parse the JSON data from the response.
4. **Error Handling**: The `try...catch` block handles any errors during the fetch operation.

---

#### **Fetching and Displaying Data**

Let’s extend the example to display specific data from the fetched results.

```javascript
async function displayUserNames() {
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        const users = await response.json();

        // Displaying user names
        users.forEach(user => {
            console.log(`Name: ${user.name}, Email: ${user.email}`);
        });
    } catch (error) {
        console.error("Error fetching or processing data:", error);
    }
}

displayUserNames();
```

**Explanation:**
- The `users.forEach` loop iterates over the array of user objects, logging each user’s name and email.

---

#### **Fetching Data Sequentially**

Sometimes you may need to make multiple requests where the next request depends on the result of the previous one. Async/await makes this straightforward.

Example: Fetch a user's data and then fetch their posts.

```javascript
async function fetchUserAndPosts(userId) {
    try {
        const userResponse = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
        if (!userResponse.ok) {
            throw new Error(`User not found! Status: ${userResponse.status}`);
        }
        const user = await userResponse.json();
        console.log(`User Name: ${user.name}`);

        const postsResponse = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`);
        if (!postsResponse.ok) {
            throw new Error(`Posts not found! Status: ${postsResponse.status}`);
        }
        const posts = await postsResponse.json();
        console.log(`Posts by ${user.name}:`);
        posts.forEach(post => console.log(`- ${post.title}`));
    } catch (error) {
        console.error("Error fetching data:", error);
    }
}

fetchUserAndPosts(1);
```

**Explanation:**
1. Fetch user data using the user’s ID.
2. Fetch posts for the user based on their ID.
3. Log the user’s name and their post titles.

---

#### **Fetching Data in Parallel**

If multiple requests are independent, you can fetch them in parallel using `Promise.all` with async/await. This improves efficiency.

Example: Fetch users and posts simultaneously.

```javascript
async function fetchUsersAndPosts() {
    try {
        const [usersResponse, postsResponse] = await Promise.all([
            fetch('https://jsonplaceholder.typicode.com/users'),
            fetch('https://jsonplaceholder.typicode.com/posts')
        ]);

        if (!usersResponse.ok || !postsResponse.ok) {
            throw new Error('Failed to fetch users or posts');
        }

        const users = await usersResponse.json();
        const posts = await postsResponse.json();

        console.log(`Fetched ${users.length} users and ${posts.length} posts.`);
    } catch (error) {
        console.error("Error fetching users or posts:", error);
    }
}

fetchUsersAndPosts();
```

**Explanation:**
- `Promise.all` takes an array of promises and waits for all of them to resolve.
- Results are destructured into `usersResponse` and `postsResponse`.
- Both sets of data are processed in parallel, reducing wait time.

---

#### **Using Async/Await to Paginate Results**

Many APIs return large datasets split into pages. Let’s fetch multiple pages of data until we reach the end.

Example: Fetch posts page by page.

```javascript
async function fetchPaginatedPosts() {
    let page = 1;
    let hasMore = true;

    while (hasMore) {
        try {
            const response = await fetch(`https://jsonplaceholder.typicode.com/posts?_page=${page}&_limit=5`);
            if (!response.ok) {
                throw new Error(`Failed to fetch page ${page}`);
            }

            const posts = await response.json();
            console.log(`Page ${page}:`);
            posts.forEach(post => console.log(`- ${post.title}`));

            if (posts.length < 5) {
                hasMore = false; // No more data
            } else {
                page++;
            }
        } catch (error) {
            console.error("Error fetching paginated posts:", error);
            break;
        }
    }
}

fetchPaginatedPosts();
```

**Explanation:**
1. The `_page` and `_limit` query parameters control pagination.
2. The `while` loop continues fetching until the number of posts returned is less than the page limit.
3. Each page’s posts are logged, and the loop stops when there’s no more data.

---

#### **Handling API Errors**

APIs can return various error codes, such as `404 Not Found` or `500 Internal Server Error`. With async/await, you can handle these gracefully.

Example: Handle errors for a non-existent endpoint.

```javascript
async function fetchNonExistentData() {
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/invalid-endpoint');
        if (!response.ok) {
            throw new Error(`Failed to fetch: ${response.status} ${response.statusText}`);
        }
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error("Error fetching data:", error);
    }
}

fetchNonExistentData();
// Output: Error fetching data: Error: Failed to fetch: 404 Not Found
```

**Explanation:**
- The `response.ok` property is checked to ensure the request succeeded.
- The `catch` block gracefully handles any errors, logging the status and reason.

---

#### **Conclusion**

Using async/await for fetching data from APIs significantly simplifies code compared to chaining promises. It makes asynchronous operations more readable and easier to debug. Whether you are fetching sequential data, performing parallel requests, or handling pagination, async/await provides a clean and powerful approach. Mastering async/await will allow you to interact with APIs efficiently, a skill essential for modern JavaScript development.