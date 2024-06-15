---
title: "Understanding Closures: A Simple Guide"
datePublished: Sat Jun 15 2024 18:48:10 GMT+0000 (Coordinated Universal Time)
cuid: clxgh0bq3000409m7ck475csi
slug: understanding-closures-a-simple-guide
canonical: https://dev.to/imkarthikeyan/one-byte-explainer-closures-o84
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/zwsHjakE_iI/upload/75b09d3a672dd101d86b2b28607cda7f.jpeg
tags: javascript, web-development, webdev

---

Hello 👋

Let's start with **Closures**

## One-Byte Explainer:

Imagine a magic box! Put your favourite toy inside, close it, & give it to a friend. Even though you can't see it, your friend can open it and play!

## Demystifying JS: Closures in action

Closures are like backpack the inner-functions carry when they exit the outerfunctions , even after the outer function's execution is destroyed.

1. **Function Inside a Function:** You have a function inside another function.
    
2. **Access to Outer Variables:** The inner function can access variables from the outer function.
    
3. **Retain Variables:** Even after the outer function finishes, the inner function keeps the outer variables.
    

Example :

```javascript

function outer() {
    let outerscope = "I am outer scope";
    
    function inner() { 
        let innerScope = "I am in inner scope";
        console.log(`${outerscope} and ${innerScope}`);
    }

    return inner;
}

const calling = outer();
calling(); // Output: "I am outer scope and I am in inner scope"
```

Thank you for reading , See you in the next blog

![final](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExbTd1c2I3NGFrYmxvYnB3ZGJqMnphYWM4eDhrODd2NHp0bHk0dms3MiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/lOJKLVYkNDWN8GoPoA/giphy.gif align="left")