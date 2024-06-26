---
title: "Publish Your Articles on Medium with NodeJS"
datePublished: Fri Apr 19 2024 15:46:06 GMT+0000 (Coordinated Universal Time)
cuid: clv6uemly000i08l33ggn6bqh
slug: publish-your-articles-on-medium-with-nodejs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1711137939086/dd5c0d69-588e-48ab-be9f-e96fd4c5c77b.png
tags: blogging, javascript, web-development, nodejs, developer, chaicode

---

## Introduction

In the Era of AI, Technical Writing is (still) one of the Key Aspects of Software Development. And Most of us post our Content on Hashnode, Medium, Dev to. But do you know you can post your articles from your NodeJS Application!!

Don't have much knowledge about that right?

No worries!

In this article,We'll explore How to post a Article to Medium from a Nodejs Application.

So, without delaying further, Let's START!

![Lets Go Let Start GIF - LetsGo LetStart LetsGetItOn GIFs](https://imgs.search.brave.com/XP0T3bOxpfAaSh1n0AMF1byWzJoA-QXGgZPWGDpOMFc/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9tZWRp/YTEudGVub3IuY29t/L2ltYWdlcy82MmIy/YzFiZTMyYWMzNjZj/NTJjYzljNTdkMzU1/NjAyZi90ZW5vci5n/aWY_aXRlbWlkPTEy/MzUyNTM5.gif align="center")

## Project Setup

### **1\. Create Node.js Project:**

To start our project, we need to set up a Node.js environment. So, let's create a node project. Run the Following command in the Terminal.

```javascript
npm init
```

This will initialize a new Node.js project.

### 2\. Install Dependencies:

Now, We'll install the required dependencies of our project.

```javascript
npm install express axios dotenv
```

This will install the following packages:

* express: a popular web framework for Node.js
    
* dotenv: loads environment variables from a `.env` file.
    
* axios: HTTP client for Node.js
    

### 3\. **Setup Environment Variables:**

Next, we'll create a `.env` folder to securely store our sensitive information such as API credentials.

```javascript
//.env
PORT = YOUR_PORT || 8000
API_KEY = YOUR_API_KEY
```

### 4\. Create Express Server:

Now, we'll create a `index.js` file in the root directory and set up a basic express server. See the following code:

```javascript
const express = require("express");
const dotenv = require("dotenv");

dotenv.config();

const app = express();

const port = process.env.PORT || 8000;

app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

Here, We're using the "dotenv" package to access the PORT number from the `.env` file.

At the top of the project, we're loading environment variables using `dotenv.config()` to make it accessible throughout the file.

### 5\. Run Project:

In this step, we'll add a start script to the `package.json` file to easily run our project.

So, Add the following script to the package.json file.

```javascript
"scripts": {
  "start": "node index.js"
}
```

The package.json file should look like this:

![Package.json file](https://cdn.hashnode.com/res/hashnode/image/upload/v1711228955682/045f024f-8f91-431b-8daf-5c792f142bdc.png align="center")

To check whether everything is working or not, let's run the project using the following command:

```javascript
npm run start
```

This will start the Express server. Now if we go to this URL [http://localhost:8000/](http://localhost:3000/) we'll get this:

![Localhost:8000/](https://cdn.hashnode.com/res/hashnode/image/upload/v1711193130560/4eb2bbb3-dd07-43ca-8005-87baf159c58e.png align="center")

Awesome! The Project setup is done and it's working perfectly. Next up, we'll add Gemini to our project in the next section

## Publishing Articles to Medium

### 1\. UserID and **API Key Generation**

At First, We'll generate the API key required for accessing the Medium API.

For that, we will go to [Settings - Medium](https://medium.com/me/settings/security) and scroll down a bit. After that, we'll get a integration token option.

![Medium Settings](https://cdn.hashnode.com/res/hashnode/image/upload/v1711177842494/51399624-78f4-48f4-b8a1-507545a015d1.png align="center")

There we'll get the API Key and update the `.env` file with the updated data.

Now, We'll Get Authenticated User Details from Medium API.

We'll Use the Medium API endpoint [`https://api.medium.com/v1/me`](https://api.medium.com/v1/me) and make a `GET` request to retrieve authenticated user data. (example: user's ID, username, name, URL, and image URL etc)

![Postman Console](https://cdn.hashnode.com/res/hashnode/image/upload/v1711178850076/8dcb4478-73f0-47d8-909d-7cb140ca21ef.png align="center")

After making the Request, We'll get the following Response:

```javascript
{
    "data": {
        "id": "MY_USER_ID",
        "username": "arindammajumder1729",
        "name": "Arindam Majumder",
        "url": "https://medium.com/@arindammajumder1729",
        "imageUrl": "https://cdn-images-1.medium.com/fit/c/400/400/1*5WOXkHCBVm7jNkNgSMmmXA.png"
    }
}
```

Now we have our necessary credentials, Let's move to the Next step!

### Implementation

The Following steps are pretty straight forward. We'll get the ApiKey and UserId from the `.env` file .

```javascript
const ApiKey = process.env.API_KEY;
const UserID = process.env.USER_ID;

const api = `Bearer ${ApiKey}`;
const url = `https://api.medium.com/v1/users/${UserID}/posts`;
```

Here,We have stored the API key, and URL in variables. This header will be used in the HTTP requests.  
Next, we'll create a `/blog` route to Publish the content to Medium!

```javascript
app.use(express.json());

app.post('/blog', async (req, res) => {
    const article = req.body;
    try {
        const response = await axios.post(
            url,
            article,
            {
                headers: {
                    "Content-Type": "application/json",
                    Authorization: api,
                },
            }
        );
        return res.status(201).send(response.data);
    } catch (error) {
        console.error(error);
        res.status(500).send('An error occurred');
    }
})
```

Here, We are taking the Article content from the Request Body and publishing the Content to Medium using the API endpoint.

With that,Our Coding Part is Done!

### **Testing:**

Now,let's see if everything is working correctly.

We'll Open Postman and make a `POST` request to [http://localhost:8000/blog](http://localhost:8000/blog) URL. With this body :

```javascript
{
     "title": "Publishing Article on Medium | by Arindam",
     "contentFormat": "markdown",
     "content": "# Here's a Basic markdown.\n***\nHope This Helps You",
     "canonicalUrl": "https://arindam1729.hashnode.dev",
     "tags": ["Medium", "Blog", "Arindam"],
     "publishStatus": "public"
}
```

And we'll get something like this:

![Postman Console- 2](https://cdn.hashnode.com/res/hashnode/image/upload/v1711186713673/35e5e45f-34ab-4847-a7c7-d0e6c406c07e.png align="center")

Awesome! We got the `201` status code that means it worked. Now Just to double check, Let's see our Medium Dashboard!

![Medium Dashboard](https://cdn.hashnode.com/res/hashnode/image/upload/v1711186785688/196d8792-7c1c-4c39-8756-44c755a65216.png align="center")

Amazing, As we can see our recently Created blog is there. That means our Code is Working properly!

For more Reference you can check this API documentation of Medium [Here](https://github.com/Medium/medium-api-docs).

## Conclusion

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web Development topics.

For Paid collaboration mail me at : [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![Thank You](https://cdn.hashnode.com/res/hashnode/image/upload/v1711136387828/8f400020-2bc9-4873-8333-3330a1a4980c.png align="center")