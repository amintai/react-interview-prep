## Question 2: Create your own `Promise.all` implementation

### **What is `Promise.all`?**

`Promise.all` is a built-in method in JavaScript that takes an array of promises and returns a single promise. This promise resolves when all the promises in the array have resolved, or it rejects as soon as one of the promises rejects.

---

### **How to Implement `Promise.all`?**

We can create our own version of `Promise.all` step by step. Here's the implementation:

```javascript
function customPromiseAll(promises) {
  return new Promise((resolve, reject) => {
    // Check if the input is an array
    if (!Array.isArray(promises)) {
      return reject(new TypeError("Input must be an array of promises"));
    }

    const results = [];
    let completedPromises = 0;

    promises.forEach((promise, index) => {
      // Convert non-promise values to resolved promises
      Promise.resolve(promise)
        .then((value) => {
          results[index] = value; // Store resolved value in order
          completedPromises++;

          // Resolve only when all promises are resolved
          if (completedPromises === promises.length) {
            resolve(results);
          }
        })
        .catch((error) => {
          // Reject as soon as one promise fails
          reject(error);
        });
    });

    // Handle case when input array is empty
    if (promises.length === 0) {
      resolve([]);
    }
  });
}
```

How It Works
Check Input Validity:

If the input is not an array, reject the promise with a TypeError.
Tracking Results:

Create an array results to store resolved values in the same order as the input.
Use completedPromises to track how many promises have resolved.
Handle Each Promise:

Loop through each promise, converting non-promise values to resolved promises using Promise.resolve().
Store the resolved value in the results array at the correct index.
If a promise is rejected, reject the entire customPromiseAll.
Empty Array Edge Case:

If the input array is empty, resolve the promise immediately with an empty array.

Example Usage:

```javascript
const promise1 = Promise.resolve(10);
const promise2 = new Promise((resolve) => setTimeout(() => resolve(20), 1000));
const promise3 = Promise.resolve(30);

customPromiseAll([promise1, promise2, promise3])
  .then((results) => {
    console.log(results); // Output: [10, 20, 30]
  })
  .catch((error) => {
    console.error(error);
  });

// Example with a rejected promise
const promise4 = Promise.reject("Error in promise");

customPromiseAll([promise1, promise4, promise3])
  .then((results) => {
    console.log(results);
  })
  .catch((error) => {
    console.error(error); // Output: "Error in promise"
  });
```

Key Features of Promise.all
Order Matters:

The order of resolved values in the result matches the order of the input promises, even if the promises resolve in a different order.
Short-Circuit on Rejection:

If any promise rejects, Promise.all immediately rejects and ignores the other promises.
Empty Input:

An empty array resolves to an empty array.

