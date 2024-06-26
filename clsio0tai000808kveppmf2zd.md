---
title: "Private Routes in React"
datePublished: Mon Feb 12 2024 08:21:31 GMT+0000 (Coordinated Universal Time)
cuid: clsio0tai000808kveppmf2zd
slug: private-routes-in-react
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1699698424727/bd6c3f21-53b5-42d6-ab5c-be75f0689408.png
tags: web-development, reactjs, beginners, reacthooks, chaicode

---

## Introduction

In modern Web Applications, ensuring the privacy and security of certain routes is a crucial aspect. We don't want everyone to access every part of our Web App. For that we use Private routes provided by React router v6.

In this Article we'll understand what is Private routes and implement it on a sample app!

Sounds interesting?

So, let's START!

## What is a Private and public Route?

Public routes are routes that are accessible to any user. They do not require authentication to access.

Private routes, on the other hand, are accessible only to authenticated users. They require the user to be logged in to access the route.

## **Project SetUp:**

For this article, I'm using a starter template from Dennis Ivy. But, you can apply the same techniques to your own project.

Here's the link to the starter template.

%[https://github.com/Arindam200/React-Private-Route-Demo/tree/main/template] 

After cloning this code, Just run `npm install` and That's it!

You can check if it's working or not by Running the following command

```bash
npm run dev
```

It will show something like this!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699699108757/6d15e42d-27c3-4176-86e1-9aa0260adaf5.png align="center")

## Implementation

In this section, we'll implement the private routes in a simple React Project! We'll be using the `Outlet` component provided by React Router 6 for that. It's like a container that contains different parts/components of our web page.

With Outlet, we can create several smaller sections, or 'sub-routes,' all within one main route.

Now we'll create a `PrivateRoutes.jsx` File within the utils folder.This checks if the user is authenticated or not before rendering the protected/private routes!

Here's the code for that:

```javascript
//utils/PrivateRoutes.jsx
import { Outlet, Navigate } from 'react-router-dom'

const PrivateRoutes = () => {
    const user = false; // Replace with your authentication logic

    return user ? <Outlet/> : <Navigate to="/login"/>
}
```

Here, If the user is authenticated it will render the private components using `<Outlet />` component otherwise it will navigate to the `/Login` route!

> Note: Change the `const user = false;` part according to your Project. For this example, I'm using a static value ie False!

Now, We'll complete the Routing setup in the `App.jsx` using the Private components that we have created before!

Here's the code for that :

```javascript
import PrivateRoutes from './utils/PrivateRoutes'

function App() {
return (
    <Router>
        <Routes>
            <Route path="/login" element={<Login/>}/>
            <Route path="/register" element={<Register/>}/>

            <Route element={<PrivateRoutes />}>
                <Route path="/" element={<Home/>}/>
                <Route path="/profile" element={<Profile/>}/>
            </Route>

        </Routes>
    </Router>
    );
}
```

Here, We have wrapped the **&lt;Home /&gt;** and **&lt;Profile /&gt;** components within the private routes. The user has to log in to access the `Home` & the `profile` component.

## Conclusion:

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web Development topics.

For Paid collaboration mail me at : [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699613881100/8836eb4c-cdd1-4406-9673-bf2111d70261.png align="center")