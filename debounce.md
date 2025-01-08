## Question 16: Create a debounce function to limit how often a task is executed.

### **What is a Debounce Function?**

A debounce function is a programming concept used to limit how often a given task is executed. It ensures that the function is called only after a specified delay period has passed since the last time it was invoked. This is particularly useful in scenarios where events are triggered in quick succession, such as resizing a window, typing in a search bar, or scrolling.

---

### **How to Create a Debounce Function in JavaScript?**

Here’s how you can implement a debounce function step by step:

```javascript
function debounce(func, delay) {
  let timer; // Holds the timeout reference

  return function (...args) {
    // Clear any previously set timeout
    clearTimeout(timer);

    // Set a new timeout to delay the function call
    timer = setTimeout(() => {
      func.apply(this, args); // Call the function with the correct context and arguments
    }, delay);
  };
}
```

How It Works
func: The function you want to debounce.
delay: The waiting time in milliseconds before executing the function.
Timer Reset: Each time the debounced function is invoked, it resets the timer and starts counting again.
Delayed Execution: The function is executed only when the timer completes without interruption.

Real-Life Example: Debouncing Window Resize Event
Let’s say we want to log a message to the console when a user stops resizing the window.

```javascript
// Create a debounced version of the function
const logResize = debounce(() => {
  console.log("Resize event handled after debounce delay!");
}, 1000); // 1000ms (1 second) delay

// Attach the debounced function to the resize event
window.addEventListener("resize", logResize);
```

Practical Use Cases
Search Boxes: Triggering an API call only after the user has stopped typing.
Scroll Events: Loading content or handling animations only after scrolling stops.
Form Validations: Validating inputs when the user pauses typing instead of on every keystroke.

Why is Debouncing Important?
Debouncing helps:

Improve performance by reducing the frequency of function calls.
Prevent unnecessary operations like repeated API requests or UI updates.
Make applications more efficient and user-friendly.
