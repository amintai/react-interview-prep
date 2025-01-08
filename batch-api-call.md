## Question 18: Group API calls in batches to reduce server load

### **Problem Statement**

You are given a list of tasks (API calls) that need to be executed. To avoid overloading the server, you want to group these API calls into smaller batches, execute them in parallel within each batch, and wait for all tasks in a batch to complete before moving on to the next batch.

---

### **Custom Implementation**

Here’s how you can implement batching for API calls:

```javascript
function executeInBatches(tasks, batchSize) {
  const results = [];

  async function processBatch(batch) {
    const batchResults = await Promise.all(batch.map(task => task()));
    results.push(...batchResults);
  }

  return (async () => {
    for (let i = 0; i < tasks.length; i += batchSize) {
      const batch = tasks.slice(i, i + batchSize);
      await processBatch(batch);
    }
    return results;
  })();
}
```

How It Works
Batch Division:

Divide the tasks array into smaller chunks of size batchSize.
Promise.all for Parallel Execution:

Execute all API calls in a single batch in parallel using Promise.all.
Sequential Batch Processing:

Use a loop to process each batch sequentially. Wait for one batch to finish before moving to the next batch using await.
Result Collection:

Collect all results from the batches into a single array, maintaining their order.

Example Usage
```javascript
// Sample API call tasks
const tasks = [
  () => new Promise((resolve) => setTimeout(() => resolve("API 1 completed"), 1000)),
  () => new Promise((resolve) => setTimeout(() => resolve("API 2 completed"), 500)),
  () => new Promise((resolve) => setTimeout(() => resolve("API 3 completed"), 300)),
  () => new Promise((resolve) => setTimeout(() => resolve("API 4 completed"), 800)),
  () => new Promise((resolve) => setTimeout(() => resolve("API 5 completed"), 1200)),
];

const batchSize = 2;

executeInBatches(tasks, batchSize)
  .then((results) => {
    console.log(results); 
    // Output: ["API 1 completed", "API 2 completed", "API 3 completed", "API 4 completed", "API 5 completed"]
  })
  .catch((error) => {
    console.error(error);
  });
```

How to Adjust Batch Size
Small Batch Size:
Useful when tasks are resource-heavy or the server has a low capacity.
Large Batch Size:
Useful when tasks are lightweight, and the server can handle more concurrent requests.

Edge Cases
1. Empty Task Array:

If the input task array is empty, the function resolves immediately with an empty array.
```javascript
executeInBatches([], 3).then((results) => console.log(results)); // Output: []
```

2. Batch Size Larger Than Tasks:

If the batchSize is larger than the number of tasks, all tasks will be executed in a single batch.
Error Handling:

If any API call fails, the batch will reject. You can handle this by wrapping the tasks in a try-catch block:

3. Error Handling:

If any API call fails, the batch will reject. You can handle this by wrapping the tasks in a try-catch block:

```javascript
const safeTask = (task) => async () => {
  try {
    return await task();
  } catch (error) {
    return `Error: ${error.message}`;
  }
};

const safeTasks = tasks.map(safeTask);
executeInBatches(safeTasks, batchSize).then(console.log);
```

Why Use Batching for API Calls?
Reduce Server Load:

Batching prevents overloading the server with too many concurrent requests.
Improved Performance:

Allows parallel processing within batches while maintaining control over concurrency.
Real-World Use Cases:

Batching is commonly used in scenarios like data fetching, uploading files, or processing large datasets.


