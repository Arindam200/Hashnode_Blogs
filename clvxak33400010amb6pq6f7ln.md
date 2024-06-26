---
title: "Getting Started With TypeScript"
seoTitle: "TypeScript Basics"
seoDescription: "Learn the basics of TypeScript, its benefits, and why it's essential for modern web development. Start your journey today!"
datePublished: Wed May 08 2024 04:00:15 GMT+0000 (Coordinated Universal Time)
cuid: clvxak33400010amb6pq6f7ln
slug: getting-started-with-typescript
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1698228958117/afcf312b-2e2e-4e52-bd25-cbd79d85a832.png
tags: javascript, developer, typescript, beginners

---

## Introduction

TypeScript is quickly becoming a fan favourite of the JavaScript community, adding the age-old concept of types to our beloved language. This dynamic addition is changing the way developers write code and is quickly becoming an essential skill in modern web development.

In this article, we'll understand What is TypeScript, its benefits & uses and many more!

So, Without any more delays, let's START!

## What is TypeScript?

According to the official docs, TypeScript is a typed superset of JavaScript. But what does that mean?

You can think of it as a layer on top of Javascript!

But Why layer?

Javascript engines can't read Typescript. So to make it readable/executable, Typescript code goes through a "pre-translation" process, called compilation.

During this compilation process, the TypeScript code is stripped of its type annotations and other TypeScript-specific constructs, resulting in plain JavaScript code. That's why We can consider typescript as "a layer" on top of JavaScript.

We use tsc (TypeScript Compiler) to convert the TypeScript code into JavaScript.

## Why TypeScript?

![Getting Started with TypeScript in React: A Comprehensive Guide](https://imgs.search.brave.com/hKjZZwymjQuO-x_J_xvjmKsjNpFWFlrVlypk_FaFsZE/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9taXJv/Lm1lZGl1bS5jb20v/djIvMSptb0plVHZX/OTd5U2hMQjdVUlJq/NUtnLnBuZw align="left")

Here comes the interesting question!

Why do we even need TypeScript? It's ultimately converting to JavaScript! So we'll directly write Javascript Code!

You also had this feeling, Right?

But let me tell you, TypeScript brings a Package of Benefits to the table.

It helps us to catch mistakes before they mess up the whole code. Also, it makes the code well-structured and **almost self-documenting.**

We'll explore TypeScript's Benefits in the next section.

## Benefits

The main benefits of using TypeScript are:

* Early error detection: TypeScript catches errors at compile time rather than runtime. This helps avoid bugs that are harder to fix later.
    
* Better code readability: Type annotations make the code more self-documenting and easier to read/understand.
    
* IntelliSense: IDEs can provide code completions and suggestions based on the static type information.
    
* Improved tooling: Tools like linters and refactors work better with static typing information.
    
* Better for large apps: Static typing scales better for large applications and codebases.
    

## Should I learn JavaScript or TypeScript?

![IntelliSense](https://imgs.search.brave.com/rpyI-wd-7Shm5-Ddw64p33hqgx82c-ktZiQjRb2JIhs/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9jb2Rl/LnZpc3VhbHN0dWRp/by5jb20vYXNzZXRz/L2RvY3MvdHlwZXNj/cmlwdC90dXRvcmlh/bC9pbnRlbGxpc2Vu/c2UucG5n align="left")

Many Developers have this question whether they should learn JavaScript or TypeScript!

Well, The answer is pretty Simple!

We can't learn TypeScript Without learning Javascript. TypeScript is basically JavaScript with extra smarts for type checking. So whatever you'll learn in Javascript will help you learn TypeScript!

## Setting up TypeScript:

The setup of TypeScript is pretty straightforward.

To install typescript globally run the Following Command in your terminal!

```bash
npm install -g typescript
```

This will install typescript globally in your system!

To verify the installation run the following command:

```bash
tsc -v
```

This will give the current version number.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1698475977902/d91c5726-b5f5-4d2a-a276-5d8e5e7d14a7.png align="center")

Now create a `file_name.ts` file and Write your first TypeScript code! To compile the TypeScript file run the following command.

```bash
tsc file_name.ts
```

This will create a `file_name.js` file and add the compiled JS code.

> If you're starting to learn Typescript, You can folllow this Series by @[Tapas Adhikary](@atapas)
> 
> %[https://www.youtube.com/playlist?list=PLIJrr73KDmRy_ufvq5m_4KwnxUdx9Sq3d] 

## Conclusion

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web Development topics.

For Paid collaboration mail me at : [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1698479265832/171faae0-7274-4310-aaa2-83e9e1d9feed.png align="center")