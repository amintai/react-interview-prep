## Question 17: Implement throttling to control the frequency of function calls

### **Problem Statement**

Throttling is a technique used to limit the number of times a function is called in a given period. Unlike debouncing, where the function is delayed until the last call, throttling ensures the function is called at most once every specified interval, even if it is triggered more frequently.

---

### **Custom Throttle Implementation**

We can implement throttling by using `setTimeout` to manage the time interval between function calls and ensure that the function is only executed once within a given timeframe.

```javascript
function throttle(callback, wait) {
  let isThrottled = false;

  return function() {
    if (isThrottled) return;

    callback.apply(this, arguments); // Call the function immediately
    isThrottled = true;

    setTimeout(() => {
      isThrottled = false; // Reset the throttle after the specified time
    }, wait);
  };
}
```

**How It Works**
1. State Tracking:
  * We use a isThrottled flag to check if the function has already been called within the current throttle window. If the flag is true, the function does not execute again.
2. Function Call:
  * If the flag is false, the function is executed immediately, and the isThrottled flag is set to true to prevent further calls within the wait period.
3. Reset the Throttle:
  * After the wait period, setTimeout resets the isThrottled flag to false, allowing the function to be called again.

**Example Usage**
```javascript
let count = 0;
const throttledFunction = throttle(() => {
  count++;
  console.log(`Function called ${count} times`);
}, 2000); // Limit function calls to once every 2 seconds

setInterval(throttledFunction, 500); // Try calling the function every 500ms
```
**Output**
```
Function called 1 times
Function called 2 times
Function called 3 times
...
```

