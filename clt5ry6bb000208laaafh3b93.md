---
title: "JWT Authentication  in NodeJS"
datePublished: Wed Feb 28 2024 12:30:08 GMT+0000 (Coordinated Universal Time)
cuid: clt5ry6bb000208laaafh3b93
slug: jwt-authentication-in-nodejs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708784263471/68915645-04ef-4a2f-90ba-a3f0e6a2bdb8.png
tags: javascript, nodejs, jwt, beginners, nodejs-developer, chaicode

---

## **Introduction**

Authentication is a crucial aspect of modern web applications. JSON Web Tokens (JWT) have become a popular method for implementing authentication in web applications because of it's simplicity, security, and scalability.

In this Article We'll discuss about What is JWT, How it works and How to use it using a sample project!

So without delaying further, Let's Get Started!

## **What is JWT?**

![JWT Image](https://imgs.search.brave.com/2_q5Noed9FMZcgmJ1yS4pnG818gHrwF9kj_gDIHHyCo/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9mcm9u/dGVnZy5jb20vd3At/Y29udGVudC93ZWJw/LWV4cHJlc3Mvd2Vi/cC1pbWFnZXMvdXBs/b2Fkcy8yMDIxLzEw/L0pXVC0xLnBuZy53/ZWJw align="left")

Before we begin let's understand what is a JWT.

So JWT, which stands for JSON Web Token, is an open standard for securely sharing JSON data between parties. The data is encoded and digitally signed, which ensures its authenticity.

JWT is widely used in API authentication and authorization workflows, as well as for data transfer between clients and servers.

### JWT Structure:

![The components of a JWT, visualized.](https://imgs.search.brave.com/85S1kIwJcUsR65JQQJAkfiBo9XVrhD2apiCVMCjEOiY/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9mdXNp/b25hdXRoLmlvL2lt/Zy9zaGFyZWQvanNv/bi13ZWItdG9rZW4u/cG5n align="left")

A JWT has three sections: a header, a payload, and a signature. Each sections are separated by periods. A typical JWT looks like this:

`xxxxxx.yyyyyy.zzzzzz`

Here the X’s represent the header, the Y’s represents the payload, and the Z’s represents the signature

An example token: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiQXJpbmRhbSIsImlhdCI6MTUxNjIzOTAyMn0._RETfYt1NardpwbQDm7z-t3Ri3ltOib8Vd6tCfyG5Nk`

1. **Header:** It's a JSON object that contains information about the token, such as the algorithm used for the signature. The header is `Base64Url` encoded. Here’s an example:
    
    ```javascript
    {
      "alg": "HS256",
      "typ": "JWT"
    }
    ```
    
2. **Paylod:** It's a JSON object containing the main data that we are trying to send, such as the user ID, permissions, and expiration time. The payload is also `Base64Url` encoded. Here’s an example
    
    ```javascript
    {
      "name": "Arindam",
      "iat": 1516239022
    }
    ```
    
3. **Signature: It** is used to verify the token’s authenticity. The signature is calculated using a secret key known only to the server, and it is typically a combination of the header and the payload, encoded with the algorithm specified in the header. Here’s an example of the signature:
    
    ```javascript
    HMAC256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret_key);
    ```
    

If you want to play with JWT and put these concepts into practice, you can use [jwt.io](http://jwt.io)[Debugger](https://jwt.io/#debugger-io) to decode, verify, and generate JWTs.

## **How does it work?**

![](https://miro.medium.com/v2/resize:fit:875/1*7T41R0dSLEzssIXPHpvimQ.png align="left")

In authentication, when the user successfully logs in using their credentials, it returns a JSON Web Token.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Note: Don't store sensitive session data in browser storage due to lack of security.</div>
</div>

And When the users want to access the protected route/private route, they have to send the JWT token typically in the Authorization header.

This works as a stateless authorization mechanism, where the server's protected routes will validate the JWT. If the JWT token is valid it sends the response to the client/user.

We'll understand it more in the next section by implementing this in the our Demo Project!

## Project Setup:

### **1\. Create Node.js Project:**

To start our project, we need to set up a Node.js environment. So, let's create a node project. Run the Following command in the Terminal.

```javascript
npm init
```

This will initialize a new Node.js project.

### 2\. Install Dependencies:

Now, We'll install the required dependencies of our project.

```javascript
npm install express mongoose jsonwebtoken dotenv
```

This will install the following packages:

* express: a popular web framework for Node.js
    
* mongoose: An ODM (Object Data Modeling) library for MongoDB.
    
* jsonwebtoken: For generating and verifying JSON Web Tokens (JWT) for authentication.
    
* dotenv: loads environment variables from a .env file
    

### 3\. **Setup Environment Variables:**

Next, we'll create a `.env` folder to securely store our sensitive information such as API credentials.

```javascript
//.env
MONGODB_URL=<Your MongoDB Connection String>
JWT_SECRET= "your_secret_key_here"
PORT=3000
```

### 4\. Creating a MongoDB Database:

First to [MongoDB Atlas](https://account.mongodb.com/account/login) and create an account or sign-in if you already have one.

Then we'll Get this Interface:

![MongoDb Atlas](https://cdn.hashnode.com/res/hashnode/image/upload/v1708613820825/98f99d2e-10e6-417b-80a0-8a1829eadb75.png align="center")

Then we'll Click on `Build a Database`, Here we'll get these options.

For this project we'll be using M0 that's the free tier.

![Pricing Page](https://cdn.hashnode.com/res/hashnode/image/upload/v1708613874806/32ba7bc9-522f-43bf-833a-8e052b59fdde.png align="center")

After selecting the Tier and the Provider we'll click button. Then we'll get something like this:

![Pricing page (2)](https://cdn.hashnode.com/res/hashnode/image/upload/v1708614243512/142e0746-9dbe-42a2-a4c7-d22c5c300d94.png align="center")

Here we'll set the username and Password. (Make sure to remember the Password)

Then Scroll a bit and set the IP to `0.0.0.0/0` and Everywhere. This means Our database can be acessible from everywhere.

![Pricing Page (3)](https://cdn.hashnode.com/res/hashnode/image/upload/v1708614350130/4b522fca-4685-422b-b019-e51738236843.png align="center")

Now we'll click the Finish button. It will initiate the deployment of our cluster.

And,We'll get something like this:

![Notification](https://cdn.hashnode.com/res/hashnode/image/upload/v1708614496393/d78659ce-ca79-4c5d-8577-78bf24cc9d5a.png align="center")

After that We'll get a dashboard like this:

![Dashboard UI](https://cdn.hashnode.com/res/hashnode/image/upload/v1708614634196/4395149b-d9df-440a-8927-ab05b800de20.png align="center")

In this Home page of the cluster,we'll click on connect button.

![Connect to Cluster Popup](https://cdn.hashnode.com/res/hashnode/image/upload/v1708614666523/81fb6130-c84a-4ff0-8d30-0a38d85607ef.png align="center")

Then, the following window will appear and then we'll click on the Compass link.

After that, we'll get something like this:

![Connection string Pop up](https://cdn.hashnode.com/res/hashnode/image/upload/v1708614707447/c322a454-1621-40c6-b760-63bcbf9fc13e.png align="center")

If you haven't installed MongoDB Compass you have to install it first. I've installed it in my machine so I have selected that.

Now We'll copy the connection string.

In my case it's `mongodb+srv://<username>:<password>@`[`cluster0.io7vzwv.mongodb.net/`](http://cluster0.io7vzwv.mongodb.net/) . Replace the username & password with your own username & password.

Now we'll Open MogoDb Compass and paste the connection string to connect with our cluster.

After Connecting we'll get this interface!

![MongoDb Compass](https://cdn.hashnode.com/res/hashnode/image/upload/v1708632618459/798319ae-2414-40a0-a5a4-ed7c761cece5.png align="center")

Awesome! we have successfully created a MongoDb database.

### 5\. Create Express Server:

Now, we'll create a `index.js` file in the root directory and set up a basic express server. See the following code:

```javascript
const express = require("express");
const dotenv = require("dotenv");
const mongoose = require("mongoose");

dotenv.config();

const app = express();
const port = process.env.PORT;

//middleware provided by Express to parse incoming JSON requests.
app.use(express.json()); 

mongoose.connect(process.env.MONGODB_URL).then(() => {
  console.log("MongoDB is connected!");
});

app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

Here, We're using the "dotenv" package to access the PORT number from the `.env` file.

At the top of the project, we're loading environment variables using `dotenv.config()` to make it accessible throughout the file.

### 6\. Run Project:

In this step, we'll add a start script to the `package.json` file to easily run our project.

By using the command `node index.js`, we have to restart your server each time when you make changes to your file. To avoid this we can install `nodemon` using the following command.

```javascript
npm install nodemon
```

So, Add the following script to the package.json file.

```javascript
"scripts": {
  "start": "nodemon index.js"
}
```

The package.json file should look like this:

![Image of Package.json file ](https://cdn.hashnode.com/res/hashnode/image/upload/v1708633924770/34e87ae3-a59f-40fa-8d74-773c942993df.png align="center")

To check whether everything is working or not, let's run the project using the following command:

```javascript
npm run start
```

This will start the Express server. Now if we go to this URL [http://localhost:3000/](http://localhost:3000/) we'll get this:

![Terminal Image after running the server](https://cdn.hashnode.com/res/hashnode/image/upload/v1708719338721/41325b0d-e24e-4f3b-b24c-4ffe6490e999.png align="center")

![Image of http://localhost:3000/](https://cdn.hashnode.com/res/hashnode/image/upload/v1707838639217/c4d08730-7534-4ad5-a0fd-5962d3eb7cc6.png align="center")

Awesome! The Project setup is done and it's working perfectly. Next up, we'll add Gemini to our project in the next section

## Authentication with JWT:

### 1\. Create User Model:

Till now we have done the basic project setup now we'll Create a new directory named “models” in our root directory and inside it, create a new file named `User.js`.

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
});

module.exports = mongoose.model("User", userSchema);
```

Here we have created a simple schema for our project.

### **2\. Create Authentication Routes:**

Next up, we'll create authentication Route for users to sign up and login.

```javascript
const express = require("express");
const jwt = require("jsonwebtoken");
const User = require("../models/User");
const router = express.Router();

// Signup route
router.post("/signup", async (req, res) => {
  try {
    const { username, password } = req.body;
    const user = new User({ username, password });
    await user.save();
    res.status(201).json({ message: "New user registered successfully" });
  } catch (error) {
    res.status(500).json({ message: "Internal server error" });
  }
});

// Login route
router.post("/login", async (req, res) => {
  const { username, password } = req.body;
  try {
    const user = await User.findOne({ username });

    if (!user) {
      return res.status(401).json({ message: "Invalid username or password" });
    }
    if (user.password !== password) {
      return res.status(401).json({ message: 'Invalid username or password' });
    }
    // Generate JWT token
    const token = jwt.sign(
      { id: user._id, username: user.username },
      process.env.JWT_SECRET
    );
    res.json({ token });
  } catch (error) {
    res.status(500).json({ message: "Internal server error" });
  }
});

module.exports = router;
```

Here, the signup route allows users to register by providing a username and password. Upon receiving the data, it creates a new user instance and saves it to the database.

And, the login route verifies the user's credentials against the database. If the username and password match, it generates a JWT token and returns it to the client.

### 3\. Add Middleware for JWT Verification:

Here we have added one middleware function `verifyJWT` that verifies the JWT token sent by the client and authenticate the user.

```javascript
const jwt = require("jsonwebtoken");

function verifyJWT(req, res, next) {
  const token = req.headers["authorization"];
  if (!token) {
    return res.status(401).json({ message: "Access denied" });
  }

  jwt.verify(token, process.env.JWT_SECRET, (err, data) => {
    if (err) {
      return res.status(401).json({ message: "Failed to authenticate token" });
    }
    req.user = data;
    next();
  });
}

module.exports = verifyJWT;
```

### 4\. **Update index.js File:**

Finally, We'll integrate these authentication routes with our Express application in the `index.js` file.

```javascript
const express = require("express");
const dotenv = require("dotenv");
const mongoose = require("mongoose");
const userMiddleware  = require("./middleware/user");
const authRouter = require("./routes/auth");

dotenv.config();

const app = express();
const port = process.env.PORT;

//middleware provided by Express to parse incoming JSON requests.
app.use(express.json()); 

mongoose.connect(process.env.MONGODB_URL).then(() => {
  console.log("MongoDB is connected!");
});

app.use('/auth', authRouter);

app.get("/protected", userMiddleware, (req, res) => {
  const { username } = req.user;
  res.send(`This is a Protected Route. Welcome ${username}`);
});

app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

Here We have created a `/protected` route which uses `userMiddleware` to checks if the user is authenticated.

If the user is authenticated , the server sends back a response saying "This is a Protected Route. Welcome \[username\]".

> Here, \[username\] is replaced with the username of the authenticated user.

### **5\. Testing the Endpoints:**

#### Signup Endpoint:

To validate the SignUp Endpoint, we'll make a POST Request to this route [http://localhost:3000/auth/signup](http://localhost:3000/auth/signup) with Headers `Content-Type : application/json` and the following JSON body:

```javascript
{
    "username": "Arindam",
    "password": "071227"
}
```

![Sign up route ](https://cdn.hashnode.com/res/hashnode/image/upload/v1708776790152/9f4f63f0-3835-4449-9433-ca5cf90b8198.png align="center")

Awesome Our Signup Endpoint is working perfectly.

#### Log In Endpoint:

To validate the LogIn Endpoint, we'll make a POST Request to this route [http://localhost:3000/auth/](http://localhost:3000/auth/signup)login with Headers `Content-Type : application/json` and the following JSON body:

```javascript
{
    "username": "Arindam",
    "password": "071227"
}
```

![Login Route Request](https://cdn.hashnode.com/res/hashnode/image/upload/v1708777258077/8b84c6d2-ab3a-4ffc-8e7a-fb7616dd1dac.png align="center")

And It's working perfectly! We got the token back as a response. We will copy the token as we'll need this token in the next section.

#### **Testing Protected Route with JWT Token:**

Now that we have obtained a JWT token from the login endpoint, we will test the protected route by including this token in the request headers.

For that, We'll Make a GET request to the protected route URL [http://localhost:3000/protected](http://localhost:3000/protected) . And In the request headers,we'll include the JWT token obtained from the login endpoint:

![/protected rote GET request](https://cdn.hashnode.com/res/hashnode/image/upload/v1708780177957/6471ef93-7ac1-49e7-b3a4-3e3c13a61c5e.png align="center")

Yay! It's Working Perfectly!!

We have successfully implemented JWT Authentication in NodeJS!

Now you can easily follow these step to authenticate your NodeJS application!

For More references You can check the following resources as well

* JWT.io: [https://jwt.io/](https://jwt.io/%EF%BF%BCMicrosoft)
    
* Microsoft Docs: [https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens](https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens%EF%BF%BCAuth0.com)
    
* Auth0.com: [https://auth0.com/learn/json-web-tokens](https://auth0.com/learn/json-web-tokens)
    

## Conclusion:

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web Development topics.

For Paid collaboration mail me at : [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![Thank You Image](https://cdn.hashnode.com/res/hashnode/image/upload/v1708704990443/81935e6d-b1a4-4cdd-9a0c-bd8ce1d5be3b.png align="center")