## Question 13: Rebuild `setInterval()` for periodic execution

### **Problem Statement**

The JavaScript `setInterval()` function is used to repeatedly execute a function at specified intervals. Your task is to recreate the behavior of `setInterval()` from scratch.

---

### **Custom Implementation**

We can achieve this by using JavaScript’s `Date` object to track the elapsed time, and `setTimeout()` to call the function repeatedly.

```javascript
function customSetInterval(callback, interval) {
  let lastTime = Date.now(); // Record the start time

  function repeat() {
    const currentTime = Date.now();
    const elapsed = currentTime - lastTime;
    
    if (elapsed >= interval) {
      callback(); // Execute the callback if the interval has passed
      lastTime = currentTime; // Update the lastTime to current time
    }

    // Continue calling the repeat function
    requestAnimationFrame(repeat); // More efficient alternative to setTimeout/setInterval
  }

  requestAnimationFrame(repeat); // Start the first call
}
```

**How It Works**
1. Track Elapsed Time:
  *The function Date.now() is used to keep track of the time when the interval starts and check the elapsed time in each cycle.
2. Efficient Looping:
  *Instead of using setInterval() directly, we use requestAnimationFrame() to repeatedly check the elapsed time. This is more efficient because it syncs with the browser’s frame updates, avoiding unnecessary function calls when the page is inactive or minimized.
3. Callback Execution:
  *If the elapsed time exceeds the interval, the callback is executed, and the lastTime is updated.
4. Repeat:
  *The requestAnimationFrame() is called recursively to continue the cycle.

**Example Usage**
```javascript
let count = 0;
customSetInterval(() => {
  console.log(`This is message number ${++count}`);
  if (count === 5) {
    console.log("Stopped after 5 messages.");
  }
}, 1000);
```
**Output**
```javasctipt
This is message number 1
This is message number 2
This is message number 3
This is message number 4
This is message number 5
Stopped after 5 messages.
```
