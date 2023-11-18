---
title: "Create  a  Custom VS Code Snippet"
datePublished: Thu Oct 19 2023 11:15:36 GMT+0000 (Coordinated Universal Time)
cuid: clnx35vjl000709i52nno5ua1
slug: create-a-custom-vs-code-snippet
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1695026351622/1b0c3211-799c-4c5a-bbd2-697760bd1549.png
tags: web-development, developer, vscode, vscode-extensions, wemakedevs

---

## Introduction:

As Developers, we always prefer not to write the same code again and again. Snippets solve this problem by making repetitive tasks simpler and quicker.

There are thousands of pre-built snippets on VS code to make our work easy. We can also create our own custom Snippet according to our preferences. In this article, we'll discuss How to create our own custom VS Code snippet.

So, Without delaying further, Let's START!!!

![Lets Start Samus Paulicelli Sticker - Lets Start Samus Paulicelli 66samus Stickers](https://imgs.search.brave.com/kWOIYLG7BcirXJd_o9SO3AgzvUSUgYdCzioLArqIo4w/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9tZWRp/YS50ZW5vci5jb20v/Z3Y3eE1GWHg1SVVB/QUFBai9sZXRzLXN0/YXJ0LXNhbXVzLXBh/dWxpY2VsbGkuZ2lm.gif align="center")

## What is Sippets?

> *A snippet is a template that can be inserted into a document. It is inserted by a command or through some trigger text.*

In simple words, with snippets we create a boilerplate file, and insert commonly used blocks of text.

## Types of Code Snippets in VS Code:

In VS Code Snippets are scoped into 3 types. They are:

1. Language Specific (Only specific language can access).
    
2. Global Snippets (Every language can access).
    
3. Project Scoped (Restricted to project).
    

## Why to use Snippets?

Before creating our custom snippets, let's see why we need snippets.

* It reduces repetitive work.
    
* Adds more reusability by allowing you to reuse the same code in multiple places.
    
* Reduces the chances of errors when copy-pasting.
    
* Increase the speed of your development.
    
* It makes your code consistent
    

## Creating Snippets:

Now we have an understanding of what is snippets and why we need them. So let's start creating our own custom VS Code Snippets.

1. At first, click on the settings icon at the bottom left of the VS code. It will open this kind of pop-up.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695014937786/9db99948-372a-4148-b0e5-71ae9848e73f.png align="center")

1. Now Click on User Snippets. Now we can choose which kind of snippets we want to create. For this example, we'll choose the Global Snippets file.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695014977727/d1940730-bb41-4561-a3ad-7fabe759a53a.png align="center")

1. Now we have to give the name of that file. Here, we'll be giving the name as `custom_snippets.json` .
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695015022931/5f95c6b5-bcd5-42ff-b092-8011ef3fc171.png align="center")

1. After that, It should look something like this if you have not set up any snippets yet.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695015566010/1833536d-5a9b-4cbe-a17f-f703f21be24c.png align="center")

1. We'll be creating a `console.log` snippet for logging a string. Here's the code for that:
    

```javascript
{
    "console.log string": {
        "prefix": "cls",
        "body": "console.log(\"$1\");",
        "description": "Custom snippet for logging a string"
    }
}
```

> If you would prefer to write a snippet in a GUI, you can use the [snippet generator web app](https://snippet-generator.app/).

**If you want to know more about the Syntax,** [**Check this**](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_snippet-syntax)**.**

1. Now we have created our Snippet! Let's see if it works or not!
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695025894650/255d4def-cd3b-4e4e-b325-dd76904dbb99.png align="center")

Yay! It works!

Now we have created our first custom VS Code snippets.

Amazing! Now you can create your own custom snippets according to your requirements!

---

## Conclusion:

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript and other web development topics.

To sponsor my work, please visit: [Arindam's Sponsor Page](https://arindam1729.hashnode.dev/sponsor) and explore the various sponsorship options.

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695025329317/220b6846-4a20-4782-ba1e-ba946723634a.png align="center")