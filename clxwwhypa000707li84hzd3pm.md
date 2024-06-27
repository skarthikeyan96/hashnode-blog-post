---
title: "AI Environmental Bot Creation: A Step-by-Step Guide with Twilio, Node.js, Gemini, and Render"
seoTitle: "Create an AI Eco Bot with Twilio & Node.js"
seoDescription: "Build an AI-driven WhatsApp environmental bot with Twilio, Node.js, Typescript, Render, and Gemini. Full step-by-step guide included"
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

Now , let's add the typescript to our project. Install the following dependencies

```bash
npm i -D typescript @types/express @types/node
```

Once done, let's initalize the `tsconfig` for our project

> `tsconfig.json` is a configuration file for TypeScript that specifies the root files and compiler options needed to compile a TypeScript project.

Run the following command in your terminal

```bash
npx tsc --init
```

This will create a basic `tsconfig.json` file in your project's root directory. You need to enable the `outDir` option, which specifies the destination directory for the compiled output. Locate this option in the `tsconfig.json` file and uncomment it.

By default, the value of this option is set to the project’s root. Change it to `dist`, as shown below:

```javascript
{
  "compilerOptions": {
    ...
    "outDir": "./dist"
    ...
  }
}
```

Let's update the `main` field in the `package.json` file to `dist/index.js` as the TypeScript code will compile from the `src` directory to `dist`. It should look something like this

```javascript
// package.json
{
  "name": "environmental-bot",
  "version": "1.0.0",
  "description": "",
  "main": "dist/index.js",
  "scripts": {
    "build": "npx tsc",
    "start": "node dist/index.js",
    "dev": "nodemon src/index.ts"
  },
    ...
}
```

Next step is to add type changes to our `index.js` file and rename it to `index.ts`

```javascript
// src/index.js -> src/index.ts

import express, { Express, Request, Response } from "express";
import dotenv from "dotenv";

dotenv.config();

const app: Express = express();
const port = process.env.PORT || 3000;

app.get("/", (req: Request, res: Response) => {
  res.send("Express + TypeScript Server");
});

app.listen(3000, () => {
  console.log('[server]: Server is running at http://localhost:3000');
});
```

Now let's try stopping and restarting the server with same command `npm run dev` and when we do , we will get the following error

```bash
Cannot use import statement outside a module
```

That is because Node.js does not natively support the execution of TypeScript files. To avoid that, let's use the package called `ts-node` .

Let's install the following dependencies and make some changes in the `package.json` script file.

```bash
npm i -D nodemon ts-node
```

> Nodemon automatically restarts your Node.js application when it detects changes in the source files, streamlining the development process.

The `build` command compiles the code into JavaScript and saves it in the `dist` directory using the TypeScript Compiler (tsc). The `dev` command runs the Express server in development mode using nodemon and ts-node.

One last step before the fun part , let's install `concurrently` and setup it in our `nodemon.json` .

> `concurrently` allows you to run multiple commands or scripts simultaneously in a single terminal window, streamlining your workflow.

```bash
## terminal command 
npm i -D concurrently

## nodemon.json
{
  "watch": ["src"],
  "ext": "ts",
  "exec": "concurrently \"npx tsc --watch\" \"ts-node src/index.ts\""
}
```

## Setting up the twilio

To complete the setup process for Twilio:

1. Log in to your Twilio account.
    
2. Access your Twilio Console dashboard.
    
3. Navigate to the WhatsApp sandbox:
    
    * Select **Messaging** from the left-hand menu.
        
    * Click on **Try It Out**.
        
    * Choose **Send A WhatsApp Message**.
        

From the sandbox tab, note the Twilio phone number and the join code. The WhatsApp sandbox allows you to test your application in a development environment without needing approval from WhatsApp.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719396378210/7717180f-35ca-4914-b71d-d0504d3da00a.png align="center")

## Coding the bot

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719397413515/8ceb0f32-2668-4b53-8a75-03d939a14cef.png align="center")

**System Architecture for AI-Driven Environmental Bot**

1. **WhatsApp Bot (UI)**: User sends their location via WhatsApp.
    
2. **OpenWeatherMap API**: Fetches the air quality index (AQI) based on the user's location.
    
3. **Gemini AI**: Analyzes the AQI data and generates personalized safety advice.
    
4. **Response**: The AI-generated advice is sent back to the user via WhatsApp.
    

To enable integration with WhatsApp, your web application must be accessible through a public domain.

To achieve this, utilize **ngrok** to expose your local web server by executing the following command in a different tab on your terminal:

```bash
ngrok http 3000
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719398681448/0bcfc9b8-dce3-4926-b5b5-16e79e09da9c.png align="center")

Add the following things to the `.env` file

```javascript
OPENWEATHERMAPAPI=****
GEMINIAPIKEY=****
```

Let's create a another endpoint `/incoming` and add it in the Sandbox settings in the twilio console. It should look something like in the below screenshot

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719398976448/1f94a6f0-2b8d-45c7-b420-1a14386d7e80.png align="center")

Add the following code in the `src/index.ts`

```javascript
import express, { Express, Request, Response } from "express";
import dotenv from "dotenv";
import twilio from 'twilio';

dotenv.config();

const app = express();
const port = process.env.PORT;
const accountSid = process.env.ACCOUNTID;
const authToken = process.env.AUTHTOKEN;
// const client = new twilio(accountSid, authToken);

app.get('/', (req: Request, res: Response) => {
  res.send('Express + TypeScript Server');
});

const MessagingResponse = twilio.twiml.MessagingResponse;

app.post('/incoming', (req, res) => {
  const message = req.body;
  console.log(`Received message from ${message.From}: ${message.Body}`);
  const twiml = new MessagingResponse();
  twiml.message(`You said: ${message.Body}`);
  res.writeHead(200, { 'Content-Type': 'text/xml' });
  res.end(twiml.toString());
});

app.listen(port, () => {
  console.log(`[server]: Server is running at http://localhost:${port}`);
});
```

Now, when you connect your WhatsApp and send a message, you should receive a reply.

![](https://instagram.fmaa1-1.fna.fbcdn.net/v/t1.15752-9/448241618_1166074791165995_3214113126363319082_n.jpg?_nc_cat=111&ccb=1-7&_nc_sid=0024fc&_nc_ohc=B1rdlwRiPmsQ7kNvgG6SWYo&_nc_ht=instagram.fmaa1-1.fna&oh=03_Q7cD1QFvSIcCdKhkzxAJqe1yCI9I6vEmMyof4hTK2Ro4PBPjGA&oe=66A35583 align="left")

Next step , let's connect the `openweathermap` and get the AQI response. Let's update the code

```javascript

// src/index.js
import express, { Express, Request, Response } from "express";
import dotenv from "dotenv";
import twilio from 'twilio';
import axios from 'axios'

dotenv.config();

const app = express();
const port = process.env.PORT;
const MessagingResponse = twilio.twiml.MessagingResponse;

async function getAirQuality(lat: number, lon: number) {
  const apiKey = process.env.OPENWEATHERMAPAPI;
  const url = `http://api.openweathermap.org/data/2.5/air_pollution?lat=${lat}&lon=${lon}&appid=${apiKey}`;
  const response = await axios.get(url);
  const airQualityIndex = response.data.list[0].main.aqi;
  return airQualityIndex;
}

app.get('/', (req: Request, res: Response) => {
  res.send('Express + TypeScript Server');
});

app.post('/incoming', async (req, res) => {
  const message = req.body;
  const {Latitude, Longtitude, From, Body} = message;
  const airQualityIndex = await getAirQuality(Latitude, Longtitude);
  console.log(`Air quality index in your places is ${airQualityIndex}`);
  const twiml = new MessagingResponse();
  twiml.message(`You said: ${message.Body}`);
  res.writeHead(200, { 'Content-Type': 'text/xml' });
  res.end(twiml.toString());
});

app.listen(port, () => {
  console.log(`[server]: Server is running at http://localhost:${port}`);
});
```

Now, when we run the following code , and send a message in whatsapp , we will get the AQI response in the console.

```javascript
                   
{
  "coord":[
    50,
    50
  ],
  "list":[
    {
      "dt":1605182400,
      "main":{
        "aqi":1
      },
      "components":{
        "co":201.94053649902344,
        "no":0.01877197064459324,
        "no2":0.7711350917816162,
        "o3":68.66455078125,
        "so2":0.6407499313354492,
        "pm2_5":0.5,
        "pm10":0.540438711643219,
        "nh3":0.12369127571582794
      }
    }
  ]
}
```

Now, for the final part, integrate Gemini to complete the bot 🤖✨.

Install the following dependencies

```bash
npm install @google/generative-ai
```

Update the following in your `src/index.ts` file

```javascript
// src/index.js
import express, { Express, Request, Response } from 'express';
import dotenv from 'dotenv';
import MessagingResponse from 'twilio/lib/twiml/MessagingResponse';
import axios from 'axios';
import { GoogleGenerativeAI } from '@google/generative-ai';
import cors from 'cors';

dotenv.config();

const app: Express = express();
const port = process.env.PORT;

const geminiAPIKey = process.env.GEMINIAPIKEY as string;
const genAI = new GoogleGenerativeAI(geminiAPIKey);
const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" });

async function getAirQuality(lat: number, lon: number) {
  const apiKey = process.env.OPENWEATHERMAPAPI;
  const url = `http://api.openweathermap.org/data/2.5/air_pollution?lat=${lat}&lon=${lon}&appid=${apiKey}`;
  const response = await axios.get(url);
  const airQualityIndex = response.data.list[0].main.aqi;

  return airQualityIndex;
}

const predictHazard = async (airQualityIndex: number) => {
  const prompt = `The air quality index is ${airQualityIndex}. The AQI scale is from 1 to 5, where 1 is good and 5 is very poor. Predict the potential hazard level and provide safety advice.`;
  const result = await model.generateContent(prompt);
  const response = await result.response;
  const text = response.text();
  console.log(text);
  return text;
}

app.use(cors());
app.use(express.urlencoded({ extended: false }));
app.use(express.json());

app.get('/', (req: Request, res: Response) => {
  res.send('Express + TypeScript Server');
});

app.post('/incoming', async (req, res) => {
  const { Latitude, Longitude, Body } = req.body;

  console.log(Latitude, Longitude);
  const airQuality = await getAirQuality(Latitude, Longitude);

  console.log("airQuality", airQuality);
  console.log(`Received message from ${Body}`);

  const alert = await predictHazard(airQuality);

  const twiml = new MessagingResponse();
  twiml.message(alert);
  res.writeHead(200, { 'Content-Type': 'text/xml' });
  res.end(twiml.toString());
});

app.listen(port, () => {
  console.log(`[server]: Server is running at http://localhost:${port}`);
});
```

Quick code overview

* **AI and API Key Setup**: Set up the Google Generative AI model and the OpenWeatherMap API key.
    
* **Air Quality Function**: Define an async function `getAirQuality` to fetch air quality data from OpenWeatherMap using latitude and longitude.
    
* **Hazard Prediction Function**: Define an async function `predictHazard` that uses the Google Generative AI model to generate safety advice based on the air quality index.
    
* **Middleware**: Use `cors`, `express.urlencoded`, and `express.json` to handle requests.
    

There are many ways to deploy the node.js application, I have connected the github repository to [render](https://render.com/).

You can start by deploying it as a service, adding the necessary environment variables, and then deploying it. Once completed, add it to the Twilio sandbox settings.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719424646851/b6cc3bd2-cc61-47a7-a8cf-d1462742beb1.png align="center")

Let's test it out.

Connect to the whatsapp sandbox and send a location message , you should get the response from the AI

![](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fcj2coquki5qmij2uftsw.png align="left")

![](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ffaye6peh7tgk907soqsm.png align="left")

## Conclusion

That's pretty much it. Thank you for taking the time to read the blog post. If you found the post useful , add ❤️ to it and let me know in the comment section if I have missed something.

Feedback on the blog is most welcome.

**Github link**: [https://github.com/skarthikeyan96/envrionmental-bot/](https://github.com/skarthikeyan96/envrionmental-bot/)

**Social Links:**

[Twitter](https://twitter.com/karthik_coder)  
[Instagram](https://www.instagram.com/coding_nemo)