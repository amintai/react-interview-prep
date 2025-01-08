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
