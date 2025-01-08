## Question 14: Design a `clearAllTimers()` function to cancel all timeouts and intervals

### **Problem Statement**

You need to design a function, `clearAllTimers()`, that cancels all active timeouts and intervals. This will be useful when you want to stop all asynchronous operations that are scheduled to run in the future.

---

### **Custom Implementation**

To clear all timeouts and intervals, we can track the IDs returned by `setTimeout()` and `setInterval()`. We store these IDs in an array and then use `clearTimeout()` and `clearInterval()` to cancel them.

```javascript
let timeouts = [];
let intervals = [];

function customSetTimeout(callback, delay) {
  const id = setTimeout(() => {
    callback();
    // Remove the timeout from the array once it has been cleared
    timeouts = timeouts.filter(timeoutId => timeoutId !== id);
  }, delay);
  timeouts.push(id);
}

function customSetInterval(callback, interval) {
  const id = setInterval(callback, interval);
  intervals.push(id);
}

function clearAllTimers() {
  // Clear all timeouts
  timeouts.forEach(id => clearTimeout(id));
  timeouts = []; // Reset timeouts array
  
  // Clear all intervals
  intervals.forEach(id => clearInterval(id));
  intervals = []; // Reset intervals array
}
```
**How It Works**
1. Track Timeout and Interval IDs:
  * Every time customSetTimeout() or customSetInterval() is called, we store the IDs returned by setTimeout() and setInterval() in separate arrays (timeouts and intervals).
2. Clearing Timers:
  * The clearAllTimers() function loops through these arrays and calls clearTimeout() and clearInterval() to cancel each timer.
3. After clearing the timers, it resets the arrays to remove the IDs.

**Example Usage**
```javascript
// Set up some timeouts and intervals
customSetTimeout(() => console.log("Timeout 1"), 1000);
customSetTimeout(() => console.log("Timeout 2"), 2000);
customSetInterval(() => console.log("Interval 1"), 500);
customSetInterval(() => console.log("Interval 2"), 1500);

// Cancel all timeouts and intervals after 3 seconds
setTimeout(() => {
  console.log("Cancelling all timers");
  clearAllTimers();
}, 3000);
```

**Output**
```
Interval 1
Interval 1
Interval 1
Interval 2
Interval 1
Interval 2
Cancelling all timers
```
