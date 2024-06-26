---
title: "Interact with AI from your Terminal with Gen-ai-chat"
seoTitle: "Chat with AI via Terminal Using Gen-ai-chat"
seoDescription: "Interact with AI directly from your terminal with Gen-ai-chat, a powerful npm package for seamless AI integration"
datePublished: Mon May 27 2024 12:10:17 GMT+0000 (Coordinated Universal Time)
cuid: clwoxfgb900000al0crsr86l7
slug: interact-with-ai-from-your-terminal-with-gen-ai-chat
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716796380339/08450243-07c8-4f5c-bb99-86f878fb2345.png
tags: ai, javascript, nodejs, devtools, cli, beginners, chaicode

---

## Introduction

Are you tired of constantly switching between different applications just to generate text? Are you looking for a way to utilize the power of AI directly from your Linux command-line interface (CLI) terminal?

If so, then this article has the perfect solution for you.

I've created [Gen-ai-chat](https://www.npmjs.com/package/gen-ai-chat), a powerful npm package that allows you to interact with AI without ever leaving your terminal.

Let's dive deep and explore how this can improve your workflow.

## **What is Gen-ai-chat?**

![Screenshot of the npm package page for "gen-ai-chat," a Google Generative AI CLI. The page includes package details such as version 0.0.5, installation instructions, repository link, and dependencies. The page has a dark theme with options for Readme, Code, Dependencies, Versions, and Settings.](https://cdn.hashnode.com/res/hashnode/image/upload/v1716754662219/bafa6fc9-00d2-43bc-b9aa-405d11aa9792.png align="center")

Gen-ai-chat is a Node.js command-line interface (CLI) tool that interacts with the Google Gemini API to generate content based on user input. With this tool, you can ask questions directly from your terminal, and get instant responses without the need to switch between different applications.

This makes it an incredibly efficient solution for developers and users who prefer working within the terminal environment.

Whether you're looking to generate text, get coding assistance, or seek answers to complex questions, `Gen-ai-chat` streamlines the process, saves you time and enhances your productivity.

## Features:

It has a variety of features designed to make your interactions with AI seamless and efficient:

1. **Direct Questions**: You can ask questions directly from the command line.
    
2. **File and Directory Context**: Provide additional context from files or entire directories.
    
3. **Interactive Mode**: Engage in a multi-question session without restarting the CLI.
    
4. **Model Selection**: Choose your favorite AI model interactively or specify it directly.
    
5. **Logging**: Write logs to a file for later review.
    
6. **Environment Variable Configuration**: Easily manage your API keys using a `.env` file.
    

## How to use it?

### Installation

To get started with Gen-ai-chat, you need to have Node.js and npm installed. You can install the package globally using npx:

```javascript
npx gen-ai-chat@latest
```

> Note: You can also use this without installing the package globally

### Basic Usage

You can start interacting with Gemini from your commandline with this simple command:

```javascript
npx gen-ai-chat "Your Question"
```

> Note: By default You can make 10 Requests/hour. You need to add your API key to use it unlimitedly

It's that simple! Right?

But wait we can do more things with this!

### **Using a File as Context**

While writing code, we often have questions or doubts. To find answers, we usually have to manually copy the code and search on different AI platforms.

However, with gen-ai-chat, you can simply mention the file where the code is located using the `-f` option, and it will do the rest of the work!

Here's an example:

```javascript
npx gen-ai-chat "Explain the code" -f ./index.js
```

This will use the `index.js` file as context and generate response based on that!

### **Using a Directory as Context**

Similar to files, if we need to use multiple files as context, we can simply mention the root directory where all the files are located using the `-d` option, and it will add all the files as context.

Here's an example usage:

```javascript
npx gen-ai-chat "Your Question" -d /path/to/your/directory
```

This will iterate through each files in the directory and add them as a context to that question.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Note: While adding files within directories, It will skip larger files such as <code>node_modules</code>, <code>package-lock.json</code> .</div>
</div>

### **Interactive Mode**

We can also use this as a chatbot and as multiple questions using the interactive mode. Start the interactive mode using the following command:

```javascript
npx gen-ai-chat -i
```

or

```javascript
npx gen-ai-chat --interactive
```

In interactive mode, the prompt `gen-ai-chat>` will appear, indicating that the tool is ready for your questions.

To exit the iteractive mode, you can just type `quit` or `exit` .

### **Choosing Your Favorite Model**

Another cool feature of gen-ai-chat is multi-model support. You can choose your favorite model interactively by using the `--choose-model` option:

```javascript
npx gen-ai-chat --choose-model
```

This command will prompt you to select a model from the available options:

![Command prompt displaying a model selection menu for "gen-ai-chat" with options: gemini-pro, gemini-1.5-flash-latest, gemini-1.5-pro-latest, gemini-pro-vision, and text-embedding-004. The currently selected option is "gemini-pro."](https://cdn.hashnode.com/res/hashnode/image/upload/v1716794840612/2c94229e-6a2f-4b29-980f-18953a970d37.png align="center")

From these options you can choose a model according to your preference.

Alternatively,You can specify the model directly using the `--model` flag:

```javascript
npx gen-ai-chat "Your question here" --model gemini-1.5-flash-latest
```

## It's Open Source: Contribute now!

Gen-ai-chat is an open-source project, and contributions are always welcome! If you have ideas for new features or improvements, feel free to open an issue on the GitHub repository.

Ready to code? Fork the repository, make your changes, and submit a pull request.

For more details and to contribute, visit the [GitHub repository](https://github.com/Arindam200/gen-ai).

## Conclusion

Overall, Gen-ai-chat provides a seamless and efficient way to integrate AI capabilities into your terminal workflow, enhancing productivity and enabling more sophisticated interactions with AI models.

Install it today and transform your workflow with the ease of AI at your fingertips

If you found this blog post helpful, please consider sharing it with others who might benefit.

For Paid collaboration mail me at : [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

!["Thank You :) text on a dark blue and purple gradient background."](https://cdn.hashnode.com/res/hashnode/image/upload/v1716796409251/c533aa24-fe79-4749-bde8-c2cd9ccfd46b.png align="center")