---
title: "How to Add Page Transition in Next.js 🧑‍💻🌠"
seoDescription: "Learn how to add smooth page transitions in Next.js using Framer Motion or the next-view-transitions library. Enhance user experience effortlessly!"
datePublished: Sat Sep 21 2024 15:14:58 GMT+0000 (Coordinated Universal Time)
cuid: cm1cajmuv000509jr0mgfalw2
slug: how-to-add-page-transition-in-nextjs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721460989527/0cf84a56-3a84-48f9-a09f-c9d330bf515b.png
tags: web-development, beginners, nextjs

---

## Introduction

Trying to Improve the user experience of your Next.js application?

One of the most effective ways to do so is by implementing smooth page transitions.

In this article, we'll explore how to add page transitions to our Next.js app using Framer Motion, a powerful React animation library.

Whether you're working on a personal portfolio, an e-commerce platform, or a complex web application, these transitions can significantly elevate the overall user experience.

So Without Delaying any Further, Let's START!

## Why to Add Page Transitions?

In Next.js Applications, By default, whenever we go to any page then it just shows the page right away without any really cool transitions which may not be a really good experience.

By Adding smooth transitions we can significantly improve the overall user experience. To Achieve optimal behavior, we have to add those transitions correctly, otherwise, it might cause unnecessary re-renders. Also, the caching capabilities of Nextjs limit us to where we can add transitions.

To implement these Transitions, we'll use Framer-motion, a popular React library, which allows us to add various animations and transitions in a performant way.

## Where to Add Page Transitions?

Now, the question is, where to add those transitions?

As mentioned in the previous section, it's very important to add transitions in the right places to ensure a smooth user experience.

![template.js special file](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Ftemplate-special-file.png&w=3840&q=75 align="left")

In Next.js, there are a few places where we can add Transitions:

1. Layouts  
    It's a shared UI between multiple routes and the topmost component. However, layouts persist across routes and maintain state, which may not be ideal for transitions.
    
2. Templates
    
    This is similar to the layout but creates a new instance for each child on navigation. This makes it an ideal spot for adding transitions, as it re-renders each time you navigate between pages.
    

For optimal performance and flexibility, the template component is generally the best place to add page transitions in Next.js

## Adding Transitions to Next.js Project

First, we need to install Framer Motion in your NextJS project. For that, run the following command:

```javascript
npm install framer-motion
```

This will install the Framer-motion library in our project.

Next, We'll create a new file called `template.tsx` in our project. This file will wrap our page content and apply the transition animations.

Now, add the following code to our newly created `template.tsx` file:

```javascript
//template.tsx
'use client';
import { motion } from 'framer-motion';

export default function Template({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      initial={{ y: 20, opacity: 0 }}
      animate={{ y: 0, opacity: 1 }}
      transition={{ type: 'spring', duration: 0.5 }}
    >
      {children}
    </motion.div>
  );
}
```

> Note:
> 
> As we've made the root component as client component, wiuldn't that make all the childrens client as well?
> 
> Well, not necessarily. we can use a children pattern to specify that not all child components need to be client components, even if the parent (template) is a client component. This pattern allows us to safely declare the root component as a client component without affecting all its children

Next, we'll update `layout.tsx` and wrap our page components with the Template component to apply the transitions.

```javascript
//layout.tsx

import type { Metadata } from "next";
import { Inter } from "next/font/google";
import "./globals.css";
import Template from './template'; // Adjust the import path as necessary

const inter = Inter({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: "Next.js with Transition",
  description: "Next.js Application with Transition made ny Arindam",
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en">
      <body className={inter.className}>
        <Template>
          {children}
        </Template>
      </body>
    </html>
  );
}
```

That's all, we've successfully added transition to our Next.js application. You can also customize the animations according to your preferences.

## Alternative Way (Using a library)

Till Now, we have seen how to implement the transition from scratch, But there's an easier way to do that.

We'll use the 'next-view-transitions' library created by [Shu Ding](https://twitter.com/shuding_) to add a smooth transition to our Next.js Application.

First, Install the package using the following command:

```javascript
npm install next-view-transitions
```

Now, Inside `layout.tsx` wrap the content using the `<ViewTransitions>` component :

```javascript
import { ViewTransitions } from 'next-view-transitions'

export default function Layout({ children }) {
  return (
    <ViewTransitions>
      <html lang='en'>
        <body>
          {children}
        </body>
      </html>
    </ViewTransitions>
  )
}
```

Finally, replace the default Next.js `<Link>` component with the one provided by `next-view-transitions` for links that should trigger a view transition:

```javascript
import { Link } from 'next-view-transitions'

export default function Component() {
  return (
    <div>
      <Link href='/about'>Go to /about</Link>
    </div>
  )
}
```

That's it, We've successfully added a smooth transition to our project. You can see one example of that [here](https://arindam-majumder.vercel.app/).

## Conclusion

In this article, we have explored how to add transitions on Next.js in two different ways both from scratch and using a library.

If you found this post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web Development topics.

For Paid collaboration mail me at: [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729), and [GitHub](https://github.com/Arindam200).

Thank you for Reading : )

![Text saying "Thank You :)" on a dark blue background with purple gradient edges.](https://cdn.hashnode.com/res/hashnode/image/upload/v1721421093425/25c521f1-df5e-4d84-918b-65189eebedf8.png align="center")