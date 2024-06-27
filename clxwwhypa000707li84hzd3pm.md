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