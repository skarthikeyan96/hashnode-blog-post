---
title: "AI-Powered Commit Message Generator - Commit-Sensei"
datePublished: Sun Jul 28 2024 09:54:20 GMT+0000 (Coordinated Universal Time)
cuid: clz5dvfn8000009ibb1m5ejqz
slug: ai-powered-commit-message-generator-commit-sensei
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/cckf4TsHAuw/upload/6f1dd7b30bfcc7766a645dd2a364e1a7.jpeg
tags: javascript, nodejs, aifortomorrow

---

In this blog, I’m thrilled to unveil **Commit-Sensei**, an interactive CLI tool designed to streamline and automate the process of generating commit messages.

## 🚀 Reason Behind Implementation:

Every time I commit code, I need to come up with a commit message that is short , fits the standards at the same time more descriptive 🤯.

I decided , why not let the AI do the process. Hence the idea was born to automate the commit process.

The next step was to research whether there were any tools that already did the same thing

![](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExbnd5M3BxMDZuOWcyY243Y2hjeDVzOG85a2gybDZqeGJkNjJnZ21yZSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/6y6fyAD9OIE6NvhJEu/giphy.gif align="center")

During my research, I discovered several tools with similar functionality. However, I noticed some gaps:

1. **Interactive Editing 📝**: Existing tools lacked the ability for users to edit commit messages before finalizing them. I wanted a tool that provides the initial AI-generated suggestion but also allows for user refinement.
    
2. **Rate Limit Awareness 🚦**: For users on a free tier, it's crucial to manage API usage effectively. A feature to notify users when they approach or exceed limits was essential.
    
3. **Personal Experimentation 🔍**: Beyond addressing these gaps, I also wanted to experiment with implementing these features, driven by a mix of curiosity and the potential for personal benefit.
    

## 🔄 Process

There was lots of learning and levels I had to cross. I'm excited to share the journey here with everyone 🌍.

**Level 1: Learning How to Build a CLI 🛠️:**

I had some experience with Node.js, but I had no idea how to build a CLI. So, level 1 was to learn how to make one. Below is the link to a video I used to learn about building a CLI using Node.js and Clack:

%[https://www.youtube.com/watch?v=GupmEQFkDJM&ab_channel=warpdotdev] 

**First level crossed!**

![Celebration GIF](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExODNvYmFpeXNtdXNpd2tkemtzbHUybHcxY2gzZWliMDRwMnl1ODEwZiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/8xomIW1DRelmo/giphy.gif align="left")

Next, I needed to break things down:

![Break it down](https://cdn.hashnode.com/res/hashnode/image/upload/v1722156850792/01cc3f81-3731-4d9e-a5f6-bc64f7814ebb.png align="left")

I was able to get through most of the steps, but got stuck with the editor part. The problem was **Clack**'s text component did not support multiline values. Whenever I plugged in the value to the text component of Clack, I was only getting the last line.

To solve this, I needed to find a component that supported multiline input. So, I switched to another package called `inquirer` to use its editor feature. I know it's not the perfect choice, but I did it!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722158442754/49699799-2530-42c3-b0a8-1bd93e96fd2e.gif align="center")

With that editor cleared on , I wanted to separate the config and the commit generation part. Next barrier came in, how to separate it out ? 🤔

![](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExZHFxeDB2eWszc283aHZ4M2IxNm9qNjd0MHg3cHVhbzAxdXBnZnFzOSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3oriOiPT4hvcvHY0Y8/giphy.gif align="center")

After some research, I discovered a package called `commander` that I could utilize. I then modified the `index.js` file to run `set-config` and `generate-commit` based on the command.

```javascript
#!/usr/bin/env node

import {program} from 'commander'
import setConfig from './set-config.js'
import generateCommit from './generate-commit.js'

program
  .command('set-config')
  .description('Set up the API key and other configurations')
  .action(setConfig);

program
  .command('generate')
  .description('Generate a commit message')
  .action(generateCommit);

program.parse(process.argv);
```

Now demo time

%[https://vimeo.com/991144317?share=copy] 

## 🛠️ Stack

* **🟩 Node.js**
    
* **💻 JavaScript (ES6+)**
    
* **📦 npm**
    
* **🤖 Google Generative AI API (Gemini Model)**
    
* **📚 Third-party Libraries**:
    
    * **🌐 @google/generative-ai**
        
    * **🎛️ @clack/prompts & @inquirer/prompts**
        
    * **🔐 dotenv**
        
    * **📁 fs & path**
        
    * **⚙️ child\_process**
        
* **📂 Configuration Files**: `.genai-config.json` & `.genai-usage.json`
    
* **🧩 Version Control**: GitHub
    
* **🔀 Git**
    

## Usage

**Installation and Configuration**

To install Commit Sensei globally, run:

```bash
npm install -g commit-sensei
```

After installation, set up the tool with your Google Generative AI API key by running:

```bash
commit-sensei set-config
```

This command prompts you to enter your API key, saving it securely in a configuration file.

**Generating Commit Messages**

With the configuration set, generating a commit message is as simple as running:

```bash
commit-sensei generate
```

This command analyzes the staged changes, generates a message, and offers an option to edit it interactively before committing.

#### Best Practices and Considerations

* **Security**: Always ensure that your `.genai-config.json` and `.genai-usage.json` files are included in your `.gitignore` file. This prevents sensitive information, such as API keys, from being exposed in your version control system.
    
* **Node.js Version**: Commit Sensei requires Node.js version 18 or higher, ensuring compatibility with the latest features and security updates.
    

## Future Roadmap

1\. **Batch Processing**

* Handle multiple commits or large diffs by summarizing changes or generating multiple commit messages.
    

2\. **Extended Emoji Support**

* Expand emoji options to cover more commit types and scenarios.
    

3\. **Customizable Prompts**

* Allow users to customize prompts and commit message formats according to their team's conventions.
    

4\. **Localization**

* Add support for multiple languages, enabling internationalization of commit messages.
    

5\. **Enhanced Configuration Management**

* Provide more robust configuration options, such as environment-based settings and secure storage.
    

6\. **Advanced Rate Limiting**

* Implement more sophisticated rate limiting strategies, including user-specific limits and real-time monitoring.
    

7\. **Integration with CI/CD**

* Integrate the tool with popular CI/CD pipelines to automate commit message generation.
    

## Conclusion

Thank you for tuning in. Application will be under continuous development. Comments and Feedbacks are most welcome. Thank you, Hashnode, for the hackathon; I learned a lot in the process.

**Github link**: [https://github.com/skarthikeyan96/commit-sensei](https://github.com/skarthikeyan96/commit-sensei)

**Social Links:**

[**Twitter**](https://twitter.com/karthik_coder)  
[**Instagram**](https://www.instagram.com/coding_nemo)

**Inspiration and References**:

1. [https://github.com/tak-bro/aicommit2](https://github.com/tak-bro/aicommit2)
    
2. [https://github.com/Nutlope/aicommits](https://github.com/Nutlope/aicommits)