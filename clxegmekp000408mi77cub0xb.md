---
title: "What Are Callbacks? An Easy Explanation"
datePublished: Fri Jun 14 2024 09:01:48 GMT+0000 (Coordinated Universal Time)
cuid: clxegmekp000408mi77cub0xb
slug: what-are-callbacks-an-easy-explanation
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/cckf4TsHAuw/upload/cb65aa28d1d2a7078d974184c6718083.jpeg
tags: javascript, web-development

---

🎉 **Welcome, folks!**

Get ready for an exciting journey through the series *Explain to a 5-Year-Old - JavaScript Concepts*.

Here's what to expect:

1. 🚀 **One-Byte Explainer:** A super concise take on the concept (256 characters or less).
    
2. 🔍 **Demystifying JS:** Dive deeper into the topic, explained clearly and engagingly.
    

---

## Callbacks

**One-Byte Explainer**:

Callbacks are like "tell me later" to a computer task. Our code does other things when the task finishes up. Then the task "calls back" with the result.

**Demystifying JS: Callbacks in Action**

Imagine you're playing a video game, but you're also hungry for pizza! You put the pizza in the oven and set a timer. Callbacks in JavaScript work similarly. They let your program keep running even when a task takes a while, just like you can keep playing the game while the pizza cooks.

Here's how it works:

1. You tell the program to do a task, like downloading a picture from the internet (similar to starting the oven).
    
2. You also give it a special instruction, like, "Hey program, when you're done downloading the picture, call me back and tell me it's ready!" This special instruction is your callback function.
    
3. Your program starts working on the download, but it doesn't wait for it to finish before doing anything else.
    
4. In the meantime, your program can keep showing you a loading screen or letting you play the game (like you can keep playing while the pizza cooks).
    
5. Once the task is finished (picture downloaded or pizza timer rings), the program "calls you back" by running your callback function. It gives you the results (the downloaded picture) so you can use them, like displaying the picture on the screen (or in this case, you can go eat the pizza!).
    

```javascript

function downloadImage(imageUrl, callback) {
  // Simulate downloading an image (replace with actual logic)
  setTimeout(() => {
    const image = "image data";  // Imagine this is the downloaded image
    callback(image);  // Call the callback function with the downloaded image
  }, 2000); // Simulate a 2 second download time

console.log("executing other tasks")
}

function displayImage(image) {
    console.log("calling the callback")
  console.log("Image downloaded and ready! Displaying:", image);
}

downloadImage("https://www.example.com/image.jpg", displayImage);
```

Thank you for reading.