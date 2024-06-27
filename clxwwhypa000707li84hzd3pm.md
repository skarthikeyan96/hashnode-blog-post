---
title: "AI Environmental Bot Creation: A Step-by-Step Guide with Twilio, Node.js, Gemini, and Render"
datePublished: Thu Jun 27 2024 06:46:06 GMT+0000 (Coordinated Universal Time)
cuid: clxwwhypa000707li84hzd3pm
slug: ai-environmental-bot-creation-a-step-by-step-guide-with-twilio-nodejs-gemini-and-render
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1719470724401/d35d9ddd-6b0e-4eac-8896-24a569f47ad5.png

---

Hello 👋

In this blog, we will explore how to build a WhatsApp bot using Twilio, Node.js, Typescript, Render, and Gemini.

**Pre-requisites :**

* Have a Twilio account. If you don’t have one, you can register for a [trial account](https://www.twilio.com/try-twilio)
    
* Install [Ngrok](https://ngrok.com/) and make sure it’s authenticated.
    
* Have a [OpenweatherMap](https://openweathermap.org/) account and the API key
    
* Create a [Gemini Account](https://aistudio.google.com/app/apikey) and the API key
    

## Setting up the development environment

First create the directory `environmental-bot` and run this following command

```bash
npm init -y
```

Install the following dependencies to setup our basic node server. Later we will add the typescript to our project.

```bash
npm i express dotenv
```

Create the index.js file in the `src` folder and the following code

```javascript
const express = require('express');
const dotenv = require('dotenv');

dotenv.config();

const app = express();
const port = process.env.PORT;

app.get('/', (req, res) => {
  res.send('Express + TypeScript Server');
});

app.listen(3000, () => {
  console.log('[server]: Server is running at http://localhost:3000');
});
```

Now in your terminal run `npm run dev` and if you check the browser , you should see the following

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719390796712/2fbb5f58-625c-4a65-a263-7ff903e35ef1.png align="center")