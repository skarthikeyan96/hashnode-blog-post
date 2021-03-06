## How to execute shell commands in Node js ?

This is a quick tutorial on how to execute `shell / windows`  commands within a nodejs application. This can come in handy when you are building a CLI which is trying to install dependencies on the other machine or running a scripts.

Alright , enough of the small talk. Let's get started

Node.js follows `Single-Threaded with Event Loop Model` and has capability of executing asynchronous task which are not handled by the main thread. When the asynchronous task is complete , output/error will return back to the main thread.

Node.js has a module called **[child_process](https://nodejs.org/api/child_process.html#child_process_child_process)** which is responsible for creating the new child process of our main Node.js process.

Two commands exec and spawn which is a method in child process that helps to execute the shell commands.

### **The exec Function:**

The `exec()` function creates a new shell and executes a given command. The output from the execution will be available for us to use in the callback.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627149314060/aS1nbN-6V.png)

Now if we execute this in our terminal , we will see the following output


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627149325822/NiaxHH-eR.png)


Now we will see how to execute the shell command with **spawn.**

### **Spawn Function:**

It creates a new process with a given command with command line args present in **args.** The output of the command is made available via listeners.   Main thing with `spawn` function is it uses stream API which  is more suitable to handle large data source.

Let's list the current working directory with **spawn**  function.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627149350545/-sfpLD5hR.png)

We setup listeners in the code once importing the child process.  **stdout** and **stderr** fires **data** event when the command writes to the stream.  Error will be thrown only if the child_process failed to execute.

Finally **close** event occurs when the command is finished.

We will be getting the following output after running in the terminal which will be same as the output we got after running the exec function.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627149370107/ofNAF05rj.png)

### **Spawn vs Exec :**

Now that we know about this two functions, when to use **spawn** and when to use **exec.**  If we are expecting large output from the command then best fit will be the **spawn function.** On the contrary if we are not expecting large output then we can go with **exec function.**

Thank you for reading. Let me know your thoughts in the comments section. 

Stay Safe and Happy Coding.
