---
title: "How to Implement Authorization in NodeJS"
seoDescription: "Learn how to implement authorization in NodeJS, including role-based access control, user schema updates, and admin route validation"
datePublished: Thu Jun 13 2024 11:37:28 GMT+0000 (Coordinated Universal Time)
cuid: clxd6qqa2000l09kz9sgkescz
slug: how-to-implement-authorization-in-nodejs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1713555090791/b7308023-ba20-41cd-aa60-dc02448d7712.png
tags: javascript, nodejs, developer, jwt, beginners, chaicode

---

## **Introduction:**

Authentication and Authorization are two fundamental concepts in web application security. As we build web applications, ensuring that only the right users can access certain parts of our site is crucial to security. This is where authentication and authorization come into play.

In this Article, We will discuss about Authentication and authorization, and How to implement them in our Nodejs Application.

So Without Delaying any further, Let’s Start!

## **Understanding Authorization**

![Close-up of a computer screen displaying HTML code with an error message stating "Authentication Failed. Please contact the administrator."](https://images.unsplash.com/photo-1618044619888-009e412ff12a?q=80&w=1000&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D align="left")

### **What Is Authorization?**

Authorization is the process of giving access to users to do specific work/actions. It decides what an authenticated user is allowed to do within the application. After Authentication, authorization makes sure that the user only does what they're allowed to do, following specific rules and permissions.

Some Common ways of Authentication are:

* Role-based access control (RBAC)
    
* Attribute-based access control (ABAC)
    
* Policy-based access control(PBAC)
    

### **How Authorization works?**

The Authorization process works like this:

* The Authenticated user makes a request to the server.
    
* The server verifies the user and gets their assigned roles and permission.
    
* The server evaluates if the user has permission for the requested actions.
    
* If the user is authorized, the server allows the user to perform the requested actions.
    
* Otherwise, it denies access and returns an appropriate response.
    

### **What is Role-based Access Control (RBAC)?**

![Diagram illustrating Role-Based Access Control (RBAC). The client sends management service requests with user credentials to the Admin Node Manager. The Admin Node Manager, which defines roles at the authority level, interacts with Service 1, Service 2, and Service 3 to perform authentication and ensure the client is authorized for access. The client then receives and processes the response.](https://lh7-us.googleusercontent.com/mpwrmZ13hD2p5SL6s64A4USQOMGxvUT46zMXkyjcHC0nKGRfF_JHIdrkOR6FWZrpmNf35a8mLMGYRow84v--hZUNjNOkxTQj3I1Yi-KCtB4O8dfHqOK3N8Ejv1Xa6-J5-rY8YQNutGpzk_jXT9-pjvg align="left")

Role-Based-Access Control is an access control method that gives permissions to the user based on their assigned roles within our applications. In simple words, It controls what users can do within the application.

Instead of assigning permissions to individual users, RBAC groups users into roles, and then assign permissions to those roles. This makes it easier to manage access control, as we only need to update permissions for a role, rather than for each individual user.

Key Components of RBAC are:

* Role: Defines a set of responsibilities, tasks, or functions within the application
    
* Permissions: Represents specific actions that users can perform.
    
* Role Assignments: Users are assigned to one or more roles, and each role is associated with a set of permissions.
    
* Access control policies: Dictate which roles can access particular resources and what actions they can take.
    

## **Implementing Authorization in NodeJS**

Till Now, We have an understanding of Authentication and Authorization. Let’s explore how to implement them in our NodeJS Application.

In this section, we’ll build a simple NodeJs app and integrate Authentication and authorization functionalities.

### Basic Project Setup and Authentication

To set up a basic Node.js project and add JWT authentication, you can check out the following article where I explain each step in detail:

[Setting up Basic Node.js Project and Add JWT Auth](https://arindam1729.hashnode.dev/jwt-authentication-in-nodejs#heading-project-setup)

### **Implementing Authorization**

In this Section, we’ll implement Authorization in our Nodejs App.

#### 1\. Update the User Schema:

We’ll update the User Schema and add one “role” field where we’ll define the role/scope of the user. We’ll use these roles in the later section for authorization.

```javascript
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,
    unique: true,
  },
  password: {
    type: String,
    required: true,
  },
  role:{
    type: String,
    required: true,
    default: "user",
  }
});

module.exports = mongoose.model("User", userSchema);
```

#### 2\. Update the Auth Route:

Next up, we’ll update the `/signup` and `/login` routes. We’ll take the newly added role from the request body in the signup route.

```javascript
// updated sign up
router.post("/signup", async (req, res) => {
  try {
    const { username, password, role } = req.body;
    const user = new User({ username, password,role });
    await user.save();
    res.status(201).json({ message: "New user registered successfully" });
  } catch (error) {
    res.status(500).json({ message: "Internal server error" });
  }
});
```

In the login route, just like username and password validation, we’ll also validate the role of the user.

```javascript
// Updated Login route
router.post("/login", async (req, res) => {
  const { username, password, role } = req.body;
  try {
    const user = await User.findOne({ username });

    if (!user) {
      return res.status(401).json({ message: "Invalid username or password" });
    }
    if (user.password !== password) {
      return res.status(401).json({ message: 'Invalid username or password' });
    }
    if (user.role !== role) {
      return res.status(401).json({ message: 'Invalid role' });
    }
    // Generate JWT token
    const token = jwt.sign(
      { id: user._id, username: user.username, role: user.role},
      process.env.JWT_SECRET
    );
    res.json({ token });
  } catch (error) {
    res.status(500).json({ message: "Internal server error" });
  }
});
```

#### 3\. Create Admin Validation middleware:

Now, we’ll create a middleware that validates if the user is an admin or not.

```javascript
function verifyAdmin(req, res, next) {
  
    if (req.user.role !== "admin") {
      return res.status(401).json({ message: "Access denied. You need an Admin role to get access." });
    }
    next();
  }

module.exports = verifyAdmin;
```

#### 4\. Create Admin route:

After that, We’ll create an admin route in our main `index.js` to validate whether the user has admin access.

```javascript
// index.js
const adminMiddleware = require("./middleware/admin");

app.get("/admin", userMiddleware, adminMiddleware, (req, res) => {
  const { username } = req.user;
  res.send(`This is an Admin Route. Welcome ${username}`);
});
```

#### 5\. Testing Endpoints:

In the signup route, we’ll create a new user with an Admin role.

For that, we’ll make a POST request to [`http://localhost:3000/auth/signup`](http://localhost:3000/auth/signup) with the following body:

```javascript
{
    "username": "Arindam Majumder",
    "password": "071204",
    "role": "admin"
}
```

![A screenshot of a Postman interface showing a POST request to "http://localhost:3000/auth/signup" with a JSON body containing a username, password, and role. The response body displays a message: "New user registered successfully" with a status of 201 Created.](https://lh7-us.googleusercontent.com/4AsIcyQYmEgP38jlExZ0JJk_nnzCLvbn6Vflg2w_fHMgnGv39JenYDBhd5W0OgOT516USD-RoPIVWqYPXBCmxMRfhUSOXcEjizs3ZELeAps6AqcB6PX7rM6Tpd1XJt2kgm0UVQ_xYmgNQTc2Cm1tKC4 align="left")

And the New User is registered. Our updated signup quote is working correctly.

Similarly, In the Login Route, we’ll make a get the token of the new user.

For that, we’ll make a GET request to [`http://localhost:3000/auth/login`](http://localhost:3000/auth/login) with a similar body:

```javascript
{
    "username": "Arindam Majumder",
    "password": "071204",
    "role": "admin"
}
```

And We’ll get a Token like this:

![A screenshot of a Postman interface showing a POST request to "http://localhost:3000/auth/login". The request body contains JSON data with fields for "username", "password", and "role". The response body displays a JSON object with a "token" field. The status of the request is 200 OK.](https://lh7-us.googleusercontent.com/H76nwPmFj-vCvnULTn0dmo2ABKjuHBx-aemMNYo9DCGTsSaKj_RpxrvdyQuENgGFN-TDefoN_-78fAenWQ3UjSeHkCpq1EIAceTWqTdbaKkvvbiTXYlrovVmYzmALcmYMakMyXw_xx7MYAm1NNWBoM8 align="left")

Now, we’ll test our admin route. For that, we’ll make a GET request to [`http://localhost:3000/admin`](http://localhost:3000/admin) with this Body:

```javascript
{
    "username": "Arindam Majumder",
    "password": "071204",
    "role": "admin"
}
```

This Time we also have to add one authorization header and add the token there to authenticate our user.

```javascript
Authorization = USERS_JWT_TOKEN
```

And we’ll get the following response:

![A screenshot of a Postman interface showing a GET request to "http://localhost:3000/admin" with JSON body parameters including "username," "password," and "role." The response section displays a message: "This is an Admin Route. Welcome Arindam Majumder." The status is 200 OK.](https://lh7-us.googleusercontent.com/hXtX9n0Bb0fJl50zJRbA50XGLPGwpKsgffdkQa_-w58p_Oo9fillBPy9f6k0jH2SOz2FOh5xwdKPLXqCcqerrrMgUpQkA2AKdqnZGN_tGjEYIiUJCqcCuibc-7Z-W-j7hlfXUOTtwf4oQQC5NvuetNA align="left")

That means our Endpoints are working as expected.  
With that, We have successfully implemented Authorization in our NodeJs Application.

## **Conclusion**

Overall, Authentication and Authorization are very important in modern-day Web applications.

In this article, we covered the basics of Authentication and Authorization and How to integrate them into the NodeJs Application.

Hope you found this helpful. Don’t forget to share your Feedback on the Comments.

Happy Coding!

![Thank You :)](https://cdn.hashnode.com/res/hashnode/image/upload/v1713555130149/625f8664-4caa-4318-bfe7-97bec77085bb.png align="center")