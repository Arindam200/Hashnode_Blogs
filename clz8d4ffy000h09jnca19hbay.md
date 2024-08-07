---
title: "How to Build a Request Access Approval System using Next.js"
seoDescription: "Learn how to build an efficient Request Access Approval System using Next.js and Permit.io for managing permissions securely and effectively"
datePublished: Tue Jul 30 2024 11:56:38 GMT+0000 (Coordinated Universal Time)
cuid: clz8d4ffy000h09jnca19hbay
slug: how-to-build-a-request-access-approval-system-using-nextjs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721460172019/b9e33379-5b1b-439d-9320-f3b5f27fc1bf.png
tags: web-development, authorization, nextjs, permit

---

## Introduction

Next.js is a powerful React framework for creating fast, scalable web applications with server-side rendering and static site generation. As applications grow, managing access to sensitive data and resources becomes essential for maintaining security and control. This is where a Request Access Approval System becomes very useful.

A Request Access Approval System enables fine-grained access control within an organization's digital infrastructure. Employees can request access to specific resources, such as confidential files or specialized software, while designated approvers review and manage these requests.

While Next.js doesn't offer built-in functionality for this system, we can integrate it using [Permit.io](http://permit.io/), a robust authorization tool. It simplifies configuring and managing permissions, making it an ideal solution for implementing complex access control in Next.js applications.

In this guide, we'll walk through building a Request Access Approval System using Next.js and [Permit.io](http://permit.io/). We'll create a demo application to demonstrate how to handle access requests, manage approvals, and enforce permissions, ensuring only authorized individuals can access sensitive information within your organization.

## What is Request Access Approval?

Before we move further, let’s understand what RAA (Request Access Approval) is. For instance, if a developer needs access to a production database, they would submit a request through the system. This request lands with a manager or an authorized person for review. They validate if the requester needs access according to their role and what is required of them.

If all is right, they accept the request. If not, they can deny it. This ensures only the correct people have legible data. This practice is called Request Access Approval.

![Flowchart showing the process of an employee access request. The steps are: "Employee Submits Access Request," "Request logged in System," "Manager Reviews Request," decision diamond "Request Approved?". If approved, the process leads to "Employee receives access notification." If denied, the outcome is "Employee receives denial notification."](https://cdn.hashnode.com/res/hashnode/image/upload/v1721454497687/dcd6f121-c1cd-44c2-8c5b-999e75465cc7.png align="center")

## Why do we need this?

So far, we have explored **RAA (Request Access Approval)**, but why do we need this? It’s a powerful way to protect sensitive data from unauthorized access, ensuring that only those needing it can see it. This reduces the risk of data breaches and helps the organization comply with various security regulations. It also helps with the process of granting access, making it faster and more efficient.

The RAA system adds an extra layer of security to protect critical information. Organizations can restrict unauthorized users from accessing sensitive data, which is essential for safeguarding customer information and other confidential data.

The RAA System streamlines the process of managing access requests. Administrators can review and approve multiple requests efficiently, and employees can quickly submit their access needs. This improves both security and efficiency within the organization.

For example, an employee might submit a request through the system, which a manager or authorized individual would then review. They consider factors like the employee's role and the sensitivity of the information before granting or denying access.

> Permit Share-if is live on ProductHunt!
> 
> Support them Here:
> 
> %[https://www.producthunt.com/posts/permit-share-if] 

## Prerequisites

To understand more about this topic, you should have a basic knowledge of the Next.js structure. You can explore more about Next.js [here](https://nextjs.org/docs).

As well, it is vital to know end-user authentication and approval. Such as session management, OAuth integration, role-based access control (RBAC), and implementing secure password hashing and storage practices.

## Step-by-Step Guide:

### Setup Next.js project

To get started, let’s create a Next.js project. To save time, we already have a starter template setup that you can use for the project.

Open a terminal and clone the GitHub repository.

```jsx
git clone <https://github.com/Arindam200/nextjs-starter>
```

This will create a Next.js project. After that, move into the newly created project folder:

```jsx
cd nextjs-starter
```

Now, we’ll Install the required dependencies

```jsx
npm install @permitio/permit-js permitio dotenv
```

Next, we’ll create a .env file in the root of our project to store our environment variables

```jsx
//.env

JWT=your_jwt_token
ENV=your_environment_key
```

> Make sure to replace `your_jwt_token` and `your_environment_key` with actual values

With that, our basic Next.js Project setup is done.

### Create an **User Management** element:

To create an access request element, it’s essential to create a **user management element first.** Let’s create it.

1. Go to Elements section
    
2. Under User Management, click on “Create Element”
    

![Screenshot of a user interface showing a sidebar menu on the left with items such as "Home," "Policy," "Directory," and "Elements," among others. The "Elements" menu item is highlighted. To the right, under "User Management," there is an option to "Create Element." Two red arrows point to "Elements" and "Create Element."](https://cdn.hashnode.com/res/hashnode/image/upload/v1721452308203/20a8b231-dabf-40bb-8b91-0d2376491044.png align="center")

1. Create a User Management Element by assigning roles as follows:
    
    1. Add a name, i.e. “**User Management**”
        
    2. Select the **RBAC** role
        
    3. Drag the **admin** role from the ”Hidden Roles” section to the “**Level 1 - Workspace Owner**”
        
    4. Create the element.
        

%[https://youtu.be/7yTrUzzoOVs] 

### Create an Access Request Element

1. Click on the Elements option
    
2. Under the Access request, click on “Create Element”
    
    ![Permit Dashboard](https://cdn.hashnode.com/res/hashnode/image/upload/v1721452416875/c542f839-0889-4f92-9174-e6c3e9cf5cf5.png align="left")
    
3. Select the user management element to which you want to connect.
    
4. Customize the element according to your choice.
    

%[https://youtu.be/iYBNFLc7jl0] 

### Implement the Access Request and User Management Element

Now we'll integrate [Permit.io](http://permit.io/)'s Access Request element and User Management element into the Next.js application.

1. First,Under the components folder, create a component `AccessRequest.tsx` for the Access Request element:
    

```javascript
import React from 'react'

function AccessRequest() {
  return (
    <iframe
            title="Permit Element AR"
            src="https://embed.permit.io/ar?envId=e0ffba3632274e8085d92c793d34f0c1&darkMode=false&tenantKey=default"
            width="100%"
            height="100%"
            style={{ border: 'none' }}
    />
  )
}

export default AccessRequest;
```

2. Next, create a component for the User Management element to manage user permissions. Add the following code in `UserManagement.tsx`
    

```javascript
:import React from 'react'

function UserManagement() {
  return (
    <iframe
        title="Permit Element user-management"
        src="https://embed.permit.io/user-management?envId=0ce2a34be877476f958a43b354e06756&darkMode=false"
        width="100%"
        height="100%"
        style="border: none;"
    />
  )
}

export default UserManagement;
```

3. Now, Create a `Dashboard.tsx` component. This component will handle the login process using [Permit.io](http://Permit.io) and embed the Access Request and User Management elements.
    

```jsx
'use client';
import permit, { LoginMethod } from '@permitio/permit-js';
import AccessRequest from './AccessRequest';
import UserManagement from './UserManagement';
import 'dotenv/config';
import { useEffect } from 'react';

const Dashboard = () => {
    useEffect(() => {
        const login = async () => {
            try {
                await permit.elements.login({
                    loginMethod: LoginMethod.frontendOnly,
                    userJwt: process.env.JWT,
                    tenant: 'default',
                    envId: process.env.ENV,
                });
                console.log('Login successful');
            } catch (err) {
                console.error('Login error:', err);
            }
        };

        if (process.env.JWT && process.env.ENV) {
            login();
        } else {
            console.error('JWT token or ENV variable is not defined.');
        }
    }, []);

    return (
        <div>
            <AccessRequest />
            <UserManagement />
        </div>
    );
};

export default Dashboard;
```

The component uses the `useEffect` hook to run a login function when the component mounts.

The login function uses `permit.elements.login()` to authenticate the user:

* It uses `LoginMethod.frontendOnly` for client-side authentication.
    
* It retrieves the JWT token and environment ID from environment variables.
    
* If login is successful, it logs a success message; otherwise, it logs an error.
    

The component renders two `iframes` that embed the [Permit.io](http://Permit.io) Access Request and User Management elements.

4. To use the `Dashboard` component, we’ll have to add the component where we want to display the access request element and user management element. For this example, add this to page.tsx :
    

```jsx
import Dashboard from "@/components/Dashboard";

export default function Home() {
  return (
    <main>
      <div >
        <Dashboard />
      </div>
    </main>
  );
}
```

All done. That’s how you can implement the [Permit.io](http://Permit.io) Request Access feature using Next.js. This is a client-side implementation. You can use server-side implementation too.

### Running the Application

To start the project, let’s run the following command in our terminal:

```jsx
npm run dev
```

This will start our project.

Now, Let’s go to [**localhost:3000**](http://localhost:3000/) \*\*,\*\*we'll get the following page:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1721747498277/9615f6e1-54e7-4de0-834a-de9fc7a0f6ac.png align="center")

Now, when we request for the access, the admin will get the following:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1721803398363/c33fe6f5-13f2-45a1-a8e9-24c67c04dffe.png align="center")

And if the admin gives access to us then we'll be able to access the restricted or private page.

With this, only authorized users access restricted areas of our application and if we give them the permission, then only they can view those contents.

## Conclusion

In this tutorial, we have explored how to implement the Request Access Approval feature in Next.js using [Permit.io](http://Permit.io). By following these steps, you can enhance the security and efficiency of access management in your applications.

If you found this post helpful, feel free to share it with others who might benefit.

Want to learn more about implementing authorization? Have questions? Join our [**Slack community**](https://io.permit.io/permitslack)**to reach out to us**.

Thank you for Reading : )