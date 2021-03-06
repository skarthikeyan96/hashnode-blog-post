## JavaScript — Call stack , event loop and callback queue


When I started as a beginner in JS, I had some difficulties in figuring out the concepts which I am about to say. I think I have figured it out. Thanks to talk given by **Philip Roberts at JS conf EU**. I will be sharing the things which I understood in the below article. This article will be about how the javascript program works under the hood.

![https://media.giphy.com/media/2GjgvS5vA6y08/giphy.gif](https://media.giphy.com/media/2GjgvS5vA6y08/giphy.gif)

First things first , what is Javascript ?

Javascript is a **single threaded**, **non-blocking**, **concurrent** and **asynchronous language**. It has single call stack and executes the program concurrently. But how ? Let’s talk about that. I will be starting with some of the terminologies which will clear your doubts.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637630893034/Tu5FTDf3Q.png)


1.  call stack
2.  callback queue
3.  event loop

**Call Stack :**

CallStack is generally a data structure which consists of active sub-routines in the computer program. So when a program executes, if it sees a **function call**, then it is **pushed onto the stack** and once the **function finishes the execution** or returns a value, then it will be **popped out from the stack**.

let’s see how a below piece of code is being executed by JS behind the scenes.

```javascript

console.log("data");

function foo(){
  console.log("foo");
}
function bar(){
  console.log("bar");
  foo();
}
bar();

```

Let's see a video which shows the execution of the program

<iframe width="560" height="315" src="https://www.youtube.com/embed/C_TMnIvABos" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

After we run the program , `console.log("data")`and since it returns value `data` it will be popped out from the stack. Followed by that, `bar()` gets pushed onto the stack which in turn prints inside `console.log()` function which is present inside the `bar()` function definition. After this `foo()` gets pushed onto the stack which in turn executes `console.log("foo")` then pops the `foo` from the stack and finally `bar` gets popped off from the stack

**Call stack ( total call stack frames 16000 )** goes out of the range in case of the recursive function call which might be caught in the endless loop.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637633082591/81rLlZfOa.png)

#### Heap:

Objects are allocated in a heap which is just a name to denote a large mostly unstructured region of memory.

#### Call back queue :

**Call back queue or message queue** contains the list of messages to be processed and their associated call back functions. The messages are queued in response of an external events ( Like response after ajax call or response from the click event ) . As the external events are web apis which are not part of the V8 runtime , when the call stack encounters it pushes to another block where it starts to execute and pushes to callback queue when it receives the response or the timer is finished.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637633101541/B1RFnlMsR.png)

As you can see from the above screenshot, set timeout function executes for `500ms` then it pushes to call back queue which it turn pushed on to the call stack by the technique called `event loop` which we will talking about soon.

Since there is no `console.log()` messages inside the call back , it will just execute the callback in the `settimeout` function after `500ms.`

#### Event loop:

Event loop is a simple piece which puts the whole puzzle together. So when the `set timeout or click function` is called or when pushed on to the stack , after the execution it goes to the callback queue. So the event loop will be checking the `call stack and the callback queue` . If the call stack is empty , then it pushes the first processed callback function present in the callback queue to the call stack. Each message is processed completely before any other message is processed.

`while (queue.waitForMessage()) {    queue.processNextMessage();   }`

`queue.waitForMessage()` waits synchronously for a message to arrive if there is none currently.

In web browsers, messages are added anytime an event occurs and there is an event listener attached to it. If there is no listener, the event is lost. So a click on an element with a click event handler will add a message — likewise with any other event.

The function `set timeout` function has two arguments which has two arguments which will be the message to add it to the queue and the time value ( default : 0 ). The time value represents the (minimum) delay after which the message will actually be pushed into the queue.

If there are no messages in the queue then , the message will be processed right after the delay. If there are messages in the queue , then it will have to wait for the previous messages to be processed. For that reason, the second **argument indicates a minimum time** and not a **guaranteed time**.



## **Conclusion:**

That's pretty much it. Thank you for taking the time to read the blog post. I hope , everyone understood how the javascript program works under the hood and also about the asynchronous part. If you found the post useful , add ❤️ to it and let me know if I have missed something in the comments section. Feedback on the blog are most welcome.


Let's connect on twitter : (**[https://twitter.com/karthik_coder](https://twitter.com/karthik_coder)**)

![https://media.giphy.com/media/eujb1tWaj3ZxS/giphy.gif](https://media.giphy.com/media/eujb1tWaj3ZxS/giphy.gif)



**Useful resources:**

1.  [loupe — Js visualisation tool by Philip roberts](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)
2.  [Event loop — mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop).
3.  [What the heck is event loop ?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
