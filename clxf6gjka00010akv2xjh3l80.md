---
title: "Understanding the Event Loop: A Simple Guide"
datePublished: Fri Jun 14 2024 21:05:05 GMT+0000 (Coordinated Universal Time)
cuid: clxf6gjka00010akv2xjh3l80
slug: understanding-the-event-loop-a-simple-guide
canonical: https://dev.to/imkarthikeyan/one-byte-explainer-event-loop-nlf
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/jLwVAUtLOAQ/upload/c098d5a0296d0128487c0a14a817515e.jpeg
tags: javascript, web-development, computer-science

---

Hello 👋

Let's start with **Event loop**

## One-Byte Explainer:

The **Event Loop** is like a traffic cop for JavaScript's code, managing a lane (callback queue) where tasks wait. The cop (Event Loop) processes tasks one by one, checking the call stack and ensuring everything runs smoothly.

## Demystifying JS: Event Loop in Action

**Event Loop** manage asynchronous operations, allowing programs to continue running without getting stuck.

Let's look at a code example and its explanation:

```javascript
console.log("data"); // Output: "data" (immediately)

for (let i = 0; i < 10; i++) {
  console.log(i); // Output: 0, 1, 2, ..., 9 (immediately, one by one)
}

setTimeout(() => {
  console.log('timer operations'); // Scheduled for later execution
}, 10);  // Reliable delay of 10 milliseconds

console.log("after setTimeout"); // Placed after setTimeout for clarity
```

> setTimeout is a function in JavaScript, but it is not a built-in function of the language itself. When you call setTimeout in your JavaScript code, the browser's JavaScript engine creates a timer in the Web API.

Here is the flow for better understanding:

![flow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cgkzmyhzwy7frn1e9jjc.png align="left")

1. "data" and the loop outputs from 0 to 9 are printed immediately.
    
2. Once the loop execution is done, the setTimeout callback is scheduled.
    
3. The timer starts as part of the Web API.
    
4. Meanwhile, the event loop keeps an eye on the callback queue for any results from asynchronous operations.
    
5. Synchronous operations continue, printing "after setTimeout".
    
6. After 10 milliseconds, the timer completes and the setTimeout callback is placed in the callback queue.
    
7. The event loop checks the call stack, and once it's empty, it pushes the first item in the queue to the stack for execution, printing "timer operations".
    

Thank you for reading , See you in the next blog

![final](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExbTd1c2I3NGFrYmxvYnB3ZGJqMnphYWM4eDhrODd2NHp0bHk0dms3MiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/lOJKLVYkNDWN8GoPoA/giphy.gif align="left")