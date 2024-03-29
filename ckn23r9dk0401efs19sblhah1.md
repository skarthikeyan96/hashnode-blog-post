## #1 Getting started with Preact - Preact Series

This is going to be three part tutorial series. In the first part , I will be explaining about Preact and how to get started. 

We will be building a preact app which will make use of API to get the images from unsplash and renders the same on the browser. 

# Preact:

**Preact** is a fast **3kb javascript library**, alternative to React with same ES6 API. Highly optimized **diff implementation** makes it one of the fastest Virtual DOM libraries. 

Since Preact only includes the subset of React's features, it makes the size of library smaller.

With regards to the browser support, all mordern browsers are supported including IE 11. 

# Advantages :

1. ** Lightweight , faster and smaller **when compared to React. You can also run performance tests in your browser via [this link](https://developit.github.io/preact-perf/)

2. Official Preact's CLI makes it easier to create projects without babel or webpack configuration. 

3. We can easily migrate from React to Preact with `preact/compat` aliasing. Below is the link to preactjs documentation about switching from react.

[Switching from React to Preact](https://preactjs.com/guide/v10/getting-started#aliasing-react-to-preact)

# What is missing in Preact ? :

1. Proptypes validation.

2. Preact uses browser's native  `addEventlistener` for event handling ignoring the synthetic events. 

**Note: **

**Synthetic events** are wrapper around the browser's native event which forms React event system. 

[Link to Synthetic events](https://reactjs.org/docs/events.html)

Let's create our preact app. 

![giphy](https://media.giphy.com/media/PiQejEf31116URju4V/giphy.gif)

## Installing Preact in local 

Run the below command in your terminal to install `preact-cli`

```shell
npm install -g preact-cli
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1617474293938/OvxbGi8lV.png)

You should be able to access the `preact` command after the successful installation of the cli. 

Run the below command to create a new `preact create` project

```
preact create default preact-unsplash
```

The above command will create a preact project named `preact-unsplash` with default settings 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1617474645083/V82RQNgGZ.png)


Now run `yarn dev` and visit [http://0.0.0.0:8080](http://0.0.0.0:8080) the link in your browser. 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1617475877286/wkuwklhU9.png)


🎉 Nicely done. Thanks for following along. Let me know in the comments if I have missed something. 

Stay tuned for part #2 of the tutorial series.

Happy Coding 🙂 

![giphy](https://media.giphy.com/media/lOJKLVYkNDWN8GoPoA/giphy.gif)


# Resources:

1. [Official documentation of Preact](https://preactjs.com)
2. [Introduction to Preact](https://blog.logrocket.com/introduction-to-preact-a-smaller-faster-react-alternative-ad5532eb6d79/)