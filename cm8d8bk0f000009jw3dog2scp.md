---
title: "10x Faster TypeScript with Go!"
datePublished: Mon Mar 17 2025 15:38:25 GMT+0000 (Coordinated Universal Time)
cuid: cm8d8bk0f000009jw3dog2scp
slug: 10x-faster-typescript-with-go
canonical: https://dev.to/arindam_1729/10x-faster-typescript-with-go-50k5
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1741985502288/f47daf5a-18a5-4b0c-89db-96c7c0896c5f.png
tags: go, typescript, go-cjffccfnf0024tjs1mcwab09t

---

The TypeScript Team recently announced they are porting to Go for better performance.

And as expected, this has created a massive discussion among the developer community.

In this Article, I’ll share all you need to know about this Hot Topic!

Let’s Start!

## What’s New?

![A 10x faster TypeScript](https://i.ytimg.com/vi/pNlq-EVld70/maxresdefault.jpg align="left")

Anders Hejlsberg, lead architect of TypeScript, announced that **starting from TypeScript v7, the compiler will be rewritten in Go**.

Till now, TypeScript uses the JavaScript Compiler, which is working well. But as the codebase grows, it faces multiple challenges like **slow compilation times, high memory usage, and long editor response delays.**

> “As your codebase grows, so does the value of TypeScript itself, but in many cases TypeScript has not been able to scale up to the very largest codebases.”  
> ~ Anders

We had to use workarounds like `tsc --watch` incremental builds, and splitting projects into smaller modules just to keep things manageable.

But with the new approach, the TypeScript team is expecting **a 10x performance improvement.** This will drastically reduce the build times, improve memory efficiency, and ensure a much smoother developer experience.

> 🎥 If You prefer video more than article, you can also check the following video:
> 
> %[https://www.youtube.com/watch?v=ltIZ4XWC6s4] 

## But Why Go?

![An introduction to why GoLang?](https://i.ytimg.com/vi/AEUw3XYjBV8/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD&rs=AOn4CLAvX-OqZOBnmYuJj6VvX4JSnzVMVA align="left")

Language choice is always a hot topic!

A lot of folks are having the same question, Why did they choose Go Language. They could have just optimized the JavaScript compiler or used a different language such as C#.

According to their [GitHub discussion,](https://github.com/microsoft/typescript-go/discussions/411) they have extensively evaluated many options, including hybrid approaches where some components could be written in a native language while keeping core type-checking algorithms in JavaScript.

They also built multiple prototypes to experiment with different data representations and studied existing native TypeScript parsers like `swc`, `oxc`, and `esbuild`.

After all experiments, Go was the suitable option for them because:

1. **Codebase Compatibility**: Go's structure closely resembles TypeScript’s JavaScript-based implementation, making it easier to maintain both versions and port changes seamlessly.
    
2. **Memory Management**: Go provides efficient memory allocation without requiring developers to micromanage garbage collection, which isn’t a significant concern in TypeScript’s use case.
    
3. **Tree Traversal & Graph Processing**: TypeScript heavily relies on tree traversal for type-checking. Go’s design makes this process more ergonomic and performant.
    
4. **Scalability & Performance**: Go’s concurrency model and efficient execution make it ideal for handling large-scale TypeScript projects.
    

Though Go’s JavaScript interop story isn’t perfect, the Team has plans to mitigate this and provide a **high-performance, ergonomic JS API**.

If you want to give it a try, you can [**build and run the Go code from their new working repo**](https://github.com/microsoft/typescript-go)**.**

## How much Faster?

The TypeScript teams have shared some real-world benchmarks comparing the current TypeScript compiler (`tsc` in JavaScript) versus the new native compiler in Go.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741981507665/b5ad5e79-c76e-4fbc-88ab-4ca9bd8ae425.png align="center")

For teams working on **large-scale TypeScript projects**, this could be a game-changer - **faster builds, smoother collaboration, and less frustration**.

## Resources

* **Official Announcement:** [Microsoft Blog](https://devblogs.microsoft.com/typescript/typescript-native-port)
    
* **Discussions & Feedback:** [GitHub Discussions](https://github.com/microsoft/typescript-go/discussions/411)
    
* **Anders Hejlsberg’s Talk:** [YouTube](https://www.youtube.com/watch?v=pNlq-EVld70)
    

---

I hope this blog gave you a clear understanding of TypeScript’s move to Go and why it matters.

If you found this article helpful, don’t forget to share it with fellow TypeScript enthusiasts.

Want more insights like this? Follow me for more deep dives into tech:

%[https://dev.to/arindam_1729] 

For Paid collaboration mail me at: [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com).

Thank you for Reading!

![GIF](https://media0.giphy.com/media/fshM9tnenoHYtvJ23B/giphy.gif?cid=6c09b952409ivvbcanle2islpg67un6ctgjqrp5mu8nwp036&ep=v1_internal_gif_by_id&rid=giphy.gif&ct=g align="left")