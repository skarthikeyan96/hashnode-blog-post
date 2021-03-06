## How to send a email using Sendgrid and Node.js ?

In this blog, We will be seeing how to send a email with Nodejs and Sendgrid mail API. 

## Pre-requisites :

1. Node and npm installed on your system

## Generating API Key on Sendgrid:

We will first need to register for a free [SendGrid account](https://signup.sendgrid.com/). 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649438501423/tB0sANneE.png)

After adding your **email-address** and **password** , Click on **Create Account.** We need to further more details to make our way through the **send-grid** dashboard.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649438520063/jZRimzd38.png)

Enter the details and click on **Get Started.  You should land on the following screen.** 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649438617095/NY3QL6iE7.png)

Before you can send any email with sendgrid , you need to **create sender identity.** 

In the sender creation form, fill up the details as follows (note that it’s better not to use a general email like Gmail):

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649438636230/WYluEaQQA.png)

Once you are finished with creating your sender identity , You need to verify the sender. 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649438659319/cILEyj1C6.png)

Head over to `API-Keys` in **settings** and click on `Create API Key` 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649438693248/iw15V9DqN.png)

Enter the name of key `Sending Email` and click on `Restricted Access` , under that click on **mail send  and enable that.**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649438714632/upRuGfOV1.png)

Once done , click on **create & view.** You should see your API key on the screen. Copy it and keep it safe, we will need that while writing code.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649438756322/zMA1XBxrk.png)

Let’s code. 

## Sending your first email :

Head over to your terminal and run the following 

```bash
mkdir sending-email-sendgrid
cd sending-email-sendgrid
npm init --y
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649438781682/JOjzcNgdC.png)

Let’s install the following packages 

```bash
yarn add dotenv @sendgrid/mail
```

Open your code editor and create .env file with following content

```json
SENDGRID_API_KEY=<PASTE THE CREATED KEY>
```

Create `index.js` file and paste the following

```jsx
const mail = require('@sendgrid/mail');
const dotenv = require("dotenv")

dotenv.config()
mail.setApiKey(process.env.SENDGRID_API_KEY);

const msg = {
  to: 'to@example.com',
  from: 'from@example.com', // Use the email address that you verified during creation of your sender identity
  subject: 'Sending my first email with Node.js',
  text: 'Email with Node js and Sendgrid',
  html: '<strong>hello world</strong>',
};

(async () => {
  try {
    await mail.send(msg);
		console.log('mail sent')
  } catch (error) {
    console.error(error);

    if (error.response) {
      console.error(error.response.body)
    }
  }
})();
```

What the above code does 

1. Importing the **sendgrid/mail sdk**  which is helpful for sending the email and configuring the `dotenv` package to access the environment variables inside our node application.
2. Configuring both **sendgrid** and **dotenv** package. 
    
    Prepping the email to send. In here for the `to`  section use the email which you verified during the sender creation
    
3. Finally using `send` method to send the mail to the user. 

Open your terminal and run the following 

```jsx
node index.js
```

You should see `mail sent` on your console. Head over to the email to check for the same. 

*Note: Check the spam folder if the email is not in your inbox*

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649438811369/Ltlqo4zpX.png)

🎉 🎉 🎉 Congratulations , You have successfully sent your email with Node.js and sendgrid. 

## Conclusion:

That's pretty much it. Thank you for taking the time to read the blog post. I hope , everyone understood how to send your first email using sendgrid and node.js.

If you found the post useful , add ❤️ to it and let me know if I have missed something in the comments section. Feedback on the blog are most welcome.

Let's connect on twitter : (**[https://twitter.com/karthik_coder](https://twitter.com/karthik_coder)**)

Repo Link: [https://github.com/skarthikeyan96/sendgrid-node-demo](https://github.com/skarthikeyan96/sendgrid-node-demo)