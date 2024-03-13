---
title: "Authenticate your React App with Appwrite"
datePublished: Wed Dec 20 2023 04:45:39 GMT+0000 (Coordinated Universal Time)
cuid: clqdaj7tr000a08l2801cc0zf
slug: authenticate-your-react-app-with-appwrite
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1698927430236/ac4be39f-b800-47ff-9692-60bbdaf25754.png
tags: authentication, javascript, web-development, developer, reactjs, beginners, appwrite

---

## Introduction

User authentication is a crucial aspect of modern web applications. Implementing user authentication from scratch is not an easy task. However, With Appwrite we can easily perform user Authentication.

In this article, We'll explore How to implement User Authentication using Appwrite in our React App.

So, Without delaying further, Let's Start!

## What is Appwrite?

![Appwrite Logo](https://appwrite.io/images/blog/logo.png align="left")

Before Jumping on the Auth Implementation let's understand what is Appwrite!

Appwrite is an open-source, self-hosted BaaS that provides a set of APIs to handle common backend needs like user authentication, file storage, databases, etc.

It aims to help developers build applications faster and more securely by abstracting away these backend tasks.

## **Project Setup:**

For this article, I'm using a starter template from Dennis Ivy. But, you can apply the same techniques to your own project.

Here's the link to the starter template.

%[https://github.com/Arindam200/React-Private-Route-Demo/tree/main/template] 

After cloning this code, Just run `npm install` and That's it!

You can check if it's working or not by Running the following command

```bash
npm run dev
```

It will show something like this!

![Sample App UI](https://cdn.hashnode.com/res/hashnode/image/upload/v1699699108757/6d15e42d-27c3-4176-86e1-9aa0260adaf5.png align="center")

## Appwrite Console Setup:

Firstly, we'll set up and configure the Appwrite console for our project!

At first, Go to the [Appwrite Console](https://cloud.appwrite.io/console/).

After Creating your account, Create a New Project.

![Appwrite dashboard](https://cdn.hashnode.com/res/hashnode/image/upload/v1698986466701/372aa049-7004-4e02-bf6a-07e88b24b50d.png align="center")

Here I'm creating One Project named "React-Auth". Now You have to choose Web as we'll be working on a Web App.

![Add a platform](https://appwrite.io/images/docs/quick-starts/dark/add-platform.png align="left")

After That, We'll Add the name of our Web app and the hostname!

Now, We'll install the Appwrite SDK using the following command:

```bash
npm install appwrite
```

So, We have installed Appwrite SDK. Next, We'll Add Appwrite to our project. For that, We'll create a `appwriteConfig.js` file in the src directory.

```javascript
//src/appwriteConfig.js

import { Client, Account } from 'appwrite';

export const API_ENDPOINT = 'https://cloud.appwrite.io/v1'
export const PROJECT_ID = 'YOUR PROJECT ID' //Replace this with your project ID

const client = new Client()
    .setEndpoint(API_ENDPOINT) 
    .setProject(PROJECT_ID);    

export const account = new Account(client);

export default client;
```

Lastly, We'll manually create a user from the Appwrite console. After Creating the user, Your console should look like this:

![Appwrite Console](https://cdn.hashnode.com/res/hashnode/image/upload/v1698992947343/10be8ac2-28ce-46cf-bbd0-b6b91b4a5721.png align="center")

Okay! So We have set up & configured the Appwrite Console. Now let's move to the Main Section!

## Authentication:

![Appwrite Authentication](https://appwrite.io/assets/visuals/auth.png align="left")

In this section, we'll do the authentication!

For that, we'll create a `AuthContext.jsx` file to manage the Auth state and different functions.

Here's the code for that:

```javascript
//utils/AuthContext.jsx
import { createContext, useState, useEffect, useContext } from "react";

const AuthContext = createContext()

export const AuthProvider = ({children}) => {

        const [loading, setLoading] = useState(true)
        const [user, setUser] = useState(null)

        useEffect(() => {
            setLoading(false)
         }, [])

         const loginUser = async (userInfo) => {}

         const logoutUser = async () => {}

         const registerUser = async (userInfo) => {}

         const checkUserStatus = async () => {}

        const contextData = {
            user,
            loginUser,
            logoutUser,
            registerUser
        }

    return(
        <AuthContext.Provider value={contextData}>
            {loading ? <p>Loading...</p> : children}
        </AuthContext.Provider>
    )
}

//Custom Hook
export const useAuth = ()=> {return useContext(AuthContext)}

export default AuthContext;
```

Here we have created AuthContext to manage the user auth state and some Functions for user authentication. Currently, these functions are empty! We'll add those in the later steps!

The `AuthProvider` component wraps the entire application with the authentication context

We have also created a custom hook **useAuth** to access this context.

Next, We will wrap the routes in `App.jsx` with `AuthProvider`.

The Code will look like this:

```javascript
...
import { AuthProvider } from './utils/AuthContext'

function App() {
    return (
        <Router>
            <AuthProvider>
                <Header/>
                <Routes>
                    ...
                </Routes>
            </AuthProvider>
        </Router>
    );
}
```

### **User Sign Up:**

Till now, We have created the Auth functions and done the basic setup! Now we'll register/sign up our user.

For that, At first, we'll Import `registerUser` from `AuthContext.jsx` that we have created before.

```javascript
//pages/Register.jsx
import { useAuth } from '../utils/AuthContext'
..
const {registerUser} = useAuth()
```

Next, We'll handle the form submission with Submithandler and ref.

Here's the code for that:

```javascript
import React, { useEffect, useRef } from 'react'
...
const registerForm = useRef(null)

const handleSubmit = (e) => {
    e.preventDefault()
    // Taking the values from the form
    const name = registerForm.current.name.value
    const email = registerForm.current.email.value
    const password1 = registerForm.current.password1.value
    const password2 = registerForm.current.password2.value
    //Password validation
    if(password1 !== password2){
        alert('Passwords did not match!')
        return 
    }
    
    const userInfo = {name, email, password1, password2}

    registerUser(userInfo)
}

...

<form ref={registerForm} onSubmit={handleSubmit}>
```

Here we have also added one check for the password confirmation, which will throw an alert if the passwords are not the same!

After that, we have taken all the values in an object (userInfo) and passed that to `registerUser` function that we have imported from `AuthContext.jsx`.

Finally, We will handle the Registration in the `AuthContext.jsx` where we have already created an empty `registerUser` function. Now we'll add functionalities to that!

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Note: To create a Unique ID for each user we'll use the {ID} provided by appwrite. ID.unique() will create a unique ID!</div>
</div>

So Now we'll add this to `AuthContext.jsx` :

```javascript
//AuthContext.jsx
import { useNavigate } from "react-router-dom";
import { ID} from 'appwrite';

...
const navigate = useNavigate()

const registerUser = async (userInfo) => {
    setLoading(true)
    try{
        let response = await account.create(ID.unique(), userInfo.email, userInfo.password1, userInfo.name);
        await account.createEmailSession(userInfo.email, userInfo.password1)
        let accountDetails = await account.get();
        setUser(accountDetails)
        navigate('/')
    }catch(error){
        console.error(error)
    }
    setLoading(false)
}
```

Cool! Now we have done the registration part! Now we can register new users.

### **User Login:**

Now, We'll work on the Login part! It's quite straightforward!

Firstly, we'll Import the User login Function from AuthContext.

```javascript
//pages/login.jsx
import { useAuth } from '../utils/AuthContext'
...

const {user, loginUser} = useAuth()
```

Next, Just like Signup, We'll handle the form submission with Submithandler and ref.

```javascript
//Import useRef
import React, { useEffect, useRef } from 'react'

//Add loginForm ref
const loginForm = useRef(null)

//Form submit handler
  const handleSubmit = (e) => {
    e.preventDefault()
    const email = loginForm.current.email.value
    const password = loginForm.current.password.value
    
    const userInfo = {email, password}

    loginUser(userInfo)
  }

//Add ref and submit function to form
<form onSubmit={handleSubmit} ref={loginForm}>
```

After that, we'll Import `account` from `appwriteConfig` and add functionality to the `loginUser` method.

```javascript
//utils/AuthContext.jsx
...
import { account } from "../appwriteConfig";
...
const loginUser = async (userInfo) => {
    setLoading(true)
    try{
        let response = await account.createEmailSession(userInfo.email, userInfo.password)
        let accountDetails = await account.get();
        setUser(accountDetails)
    }catch(error){
        console.error(error)
    }
    setLoading(false)
}
```

As simple as that! We have done the Login part! Now we can `LogIn` our user!

### Checking User Status:

We can check the user status by calling the checkUserStatus method. It will now update the user and loading state from the useEffect hook.

```javascript
//utils/AuthContext.jsx
useEffect(() => {
    //setLoading(false)
    checkUserStatus()
}, [])
const checkUserStatus = async () => {
    try{
        let accountDetails = await account.get();
        setUser(accountDetails)
    }catch(error){  
    }
    setLoading(false)
}
```

### **User LogOut:**

So, We have done SignUp and LogIn methods! Now we'll add the final part of our Authentication which is Logout!

It's very easy!

```javascript
//components/Header.jsx
const {user, logoutUser} = useAuth()

//trigger the logoutUser method
<button onClick={logoutUser} className="btn">Logout</button>
```

For logging out the user we will use `account.deleteSession` method. It will delete the current session of the user.

```javascript
//utils/AuthContext.jsx
const logoutUser = async () => {
    await account.deleteSession('current');
    setUser(null)
}
```

And That's it! We have Completed our Authentication part!

Now, you can follow the same steps to authenticate your React app with Appwrite.

Easy peasy!

## Conclusion

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web Development topics.

For Paid collaboration mail me at : [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![Thank You Image](https://cdn.hashnode.com/res/hashnode/image/upload/v1698959508804/b205f50a-9f8e-4778-ae3b-8979b4c909c8.png align="center")