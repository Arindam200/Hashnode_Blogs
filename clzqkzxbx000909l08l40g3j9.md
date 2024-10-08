---
title: "I Built an Event Scheduler in NodeJs"
seoDescription: "Learn how to create a Node.js event scheduler with Google Calendar integration and email invites in this step-by-step guide"
datePublished: Mon Aug 12 2024 05:56:56 GMT+0000 (Coordinated Universal Time)
cuid: clzqkzxbx000909l08l40g3j9
slug: event-scheduler-in-nodejs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723241178114/0f0a39fd-09df-4c06-96ab-44f25f24b6bd.gif
tags: javascript, nodejs, developer, google, beginners

---

## Introduction

Since COVID, my calendar has been full of stand-ups, team meetings, and client calls.

However, scheduling an event and inviting guests are boring tasks. One Friday, after spending too much time on these, I thought –

Why am I spending so much time on this?

![frustrated seriously GIF](https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExdXowZngxaGxqcHIzbHM2N2Q0eDZ1M3JzZXRuMzY3anI3aXZyazEwdCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/nkLB4Gp8H6hFe/giphy.gif align="left")

Thus, I had the idea to create an Event scheduler to simplify my work!

In this article, I'll show you how to create a Nodejs application that can create events and automatically send out email invites with Google Meet links.

Excited? Me too.

So without Delaying any further!

Let's START!

## Project Setup:

### **1\. Create Node.js Project:**

To start our project, we need to set up a Node.js environment. So, let's create a node project. Run the following command in the Terminal.

```javascript
npm init -y
```

This will initialize a new Node.js project.

### 2\. Install Dependencies:

Now, We'll install the required dependencies of our project.

```javascript
npm install express googleapis dotenv
```

This will install the following packages:

* express: a popular web framework for Node.js
    
* dotenv: loads environment variables from a `.env` file.
    
* googleapis: Provides access to various Google APIs
    

### 3\. **Setup Environment Variables:**

Next, we'll create a `.env` folder to securely store our sensitive information such as API credentials.

```javascript
//.env
PORT = YOUR_PORT || 8000
CLIENT_ID = YOUR_CLIENT_ID
CLIENT_SECRET = YOUR_CLIENT_SECRET
REDIRECT_URL = http://localhost:8000/auth/redirect
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

## Setting Up Google Console

At First, We'll go to the [Google Cloud Console](https://console.cloud.google.com/).

Then we'll get this Dashboard. (I have previously created one project that's why I'm getting this, you might get something else)

![Google Cloud Console](https://cdn.hashnode.com/res/hashnode/image/upload/v1709442856354/2e8cad60-1f22-4257-aa49-1d3226bac646.png align="center")

Now, We'll click on the 'New Project' button to start a new project.

![New Project Page](https://cdn.hashnode.com/res/hashnode/image/upload/v1709442899939/4c93f8af-c89d-48a8-a7a9-a6d20c71d7dc.png align="center")

Next up we'll get something like this. Here we'll add our Project's name and organisation.  
For this project, I'm keeping this as "Mail-integration-Demo". Then we'll Click the Create button to proceed

![New Project Details](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443035240/1464fb60-e84e-4c5b-8c09-2c0b87df3cb6.png align="center")

Next, In the Side Navigation bar, we'll get "APIs and Services". Within this section, there's a submenu to enable APIs and services. we'll click that to proceed.

![APIs and Services](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443074431/45a3cd82-faa6-4f48-8550-1aa808cd4ec2.png align="center")

Next, We'll enable the API that we'll be using in this project ie. the Google Calender API.

![Google Calendar API](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443147501/26efa3a0-074b-42e7-be6b-9ce96172a668.png align="center")

After that, we'll go to the OAuth Consent Screen. Here we'll select the User Type as External. And we'll press Create button to proceed.

![OAuth consent screen](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443233572/6eef25e0-8cfe-4957-a86d-9eebb1ea8afe.png align="center")

Then we'll go to the app registration page.  
Here we'll be adding more information about our application. We start by adding the name of our app and an email address for user support.

For this project, I'll name it "Arindam's Mail Integration" and use my own email address for support.

![App information page](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443315446/7db1dbca-a458-4d8e-aaa1-a70137e3554d.png align="center")

Next, we have to define the scope of the Application

![Scopes page](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443339724/9cf72f9b-70b8-4d1f-983e-01cf529d0d69.png align="center")

In the Scopes, We'll add necessary permissions such as `userinfo.email` and `userinfo.profile` for this project.

![selected scopes](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443363123/8f644e6b-7026-44a8-b527-53f671ae995e.png align="center")

After that, We will add one test user to our Application.

![Test User page](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443406970/28df0e46-aaa7-4735-b6b2-2fccc82070b9.png align="center")

That's it. Our Application is registered with the platform.

Now, We'll create our OAuth Client ID secret. For that, we'll go over to the Create Credential part.

![Google Console Dashboard](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443465484/ce015786-1e22-47fd-81da-17c88e1f31b1.png align="center")

Here we'll add the type of our Apllication and It's name. For this project its web application and the name is Arindam's Mail Demo.

![OAuth client ID](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443522426/b1deea68-54d5-4b08-9ec1-71aa948d49a0.png align="center")

Also, we have added one Redirect URL. For this project, it's going to be `http://localhost:8000/auth/redirect`.

![Redirect URLs](https://cdn.hashnode.com/res/hashnode/image/upload/v1709443613884/b258fe29-72bc-4959-9d1a-e312307e5a6e.png align="center")

And then we'll get the OAuth credential.

![OAuth client created](https://cdn.hashnode.com/res/hashnode/image/upload/v1709444437957/4edfbf84-6608-4ec0-85e3-3d271ff24fdc.png align="center")

Next up we'll create API Keys.

![API Key Generation Page](https://cdn.hashnode.com/res/hashnode/image/upload/v1709444545223/7449ce26-1e78-4a5c-8fec-c16441192fb6.png align="center")

After doing all this we'll Update our `.env` file with the API keys and the OAuth Credentials that we have generated earlier.

With this, we have set up our Google Cloud console for this project, now let's move to the next section

## OAuth 2 Authentication:

Till now we have done our basic project setup. Now, we'll integrate OAuth2 Authentication into our Project. This enables our application to interact securely with Google services.  
For that, First We'll import the required packages into the `index.js` file.

```javascript
const express = require('express');
const { google } = require('googleapis');
const dotenv = require('dotenv');
```

Then we'll define the scope of access required for the Google Calendar API.

```javascript
const scopes = ['https://www.googleapis.com/auth/calendar'];
```

Next, We'll configure the OAuth 2 client using the credentials that we have stored in the `.env` file.

```javascript
// OAuth 2 configuration
const oauth2Client = new google.auth.OAuth2
(
    process.env.CLIENT_ID,
    process.env.CLIENT_SECRET,
    process.env.REDIRECT_URL
);
```

After the OAuth 2 Configuration, We'll create a Route to Authenticate our Users.

```javascript
app.get('/auth', (req, res) => {

    const url = oauth2Client.generateAuthUrl
    ({
        access_type: 'offline',
        scope: scopes
    });
    res.redirect(url);
    }
);
```

When our users go to this route, they'll be redirected to Google's authentication page where it will ask for specific permissions to our application.

After Successful authentication, Google will redirect the user to our Redirect URL (`/auth/redirect`)

```javascript
app.get("/auth/redirect", async (req, res) => {

    const {tokens} = await oauth2Client.getToken(req.query.code);
    oauth2Client.setCredentials(tokens);
    res.send('Authentication successful! Please return to the console.');
    }

);
```

Here we are getting the refresh tokens from the Query and storing them as credentials in the `oauth2Client`. These credentials will help us to make request to the Google Calendar API.

Here's the complete code for `index.js` :

```javascript
//index.js
const express = require('express');
const { google } = require('googleapis');
const dotenv = require('dotenv');

const app = express();

dotenv.config();

const port = process.env.PORT || 8000;

app.get('/', (req, res) => {
    res.send('Hello World');
    }
);

// Define the scope of access for the Google Calendar API.
const scopes = ['https://www.googleapis.com/auth/calendar'];

// OAuth 2 configuration
const oauth2Client = new google.auth.OAuth2
(
    process.env.CLIENT_ID,
    process.env.CLIENT_SECRET,
    process.env.REDIRECT_URL
); 

app.get('/auth', (req, res) => {

    const url = oauth2Client.generateAuthUrl
    ({
        access_type: 'offline',
        scope: scopes
    });
    res.redirect(url);
    }
);

app.get("/auth/redirect", async (req, res) => {

    const {tokens} = await oauth2Client.getToken(req.query.code);
    oauth2Client.setCredentials(tokens);
    res.send('Authentication successful! Please return to the console.');
    }

);
```

## Scheduling Events on Google Calendar

Here comes the most important part! In this Section, we'll be scheduling an event on Google Calendar!

To begin, We'll initialize the Google Calendar API client.

```javascript
const calendar = google.calendar({
    version: 'v3', 
    auth: oauth2Client
});
```

Next, We'll create an event object where we'll add all the details of the event such as the Summary, Location, Start and End time, etc.

```javascript
const event = {
    summary: 'Tech Talk with Arindam',
    location: 'Google Meet',

    description: "Demo event for Arindam's Blog Post.",
    start: {
        dateTime: "2024-03-14T19:30:00+05:30",
        timeZone: 'Asia/Kolkata'
    },
    end: {
        dateTime: "2024-03-14T20:30:00+05:30",
        timeZone: 'Asia/Kolkata'
    },
};
```

After that we'll create a Route (`/create-event` ) where we'll create the event.

Here we are inserting an event in the user's Calendar using the `calendar.events.insert()` method.

```javascript
app.get('/create-event', async (req, res) => {

    try {
        const result = await calendar.events.insert({
                                calendarId: 'primary', 
                                auth:oauth2Client,
                                resource: event
                        });

        res.send({
            status: 200,
            message: 'Event created',
        });
    } catch (err) {
        console.log(err);
        res.send(err);
    }
    }
);
```

With this, we can dynamically schedule events on Google Calendar.

### Adding Google Meet Link:

Till now, we have explored how to create a simple event on Google Calendar. In this section, we'll explore how to add a Google Meet Link to that Event!

For that, we'll be updating the event object that we have created in the previous section. we'll add a `conferenceData` property to specify the creation request for a Google Meet link.

```javascript
 conferenceData: {
            createRequest: {
                requestId: uuid(),
            }
        },
```

We'll also add an `attendees` property to invite guests to the Event. Here's a simple example of that:

```javascript
attendees: [
            {email: 'arindammajumder2020@gmail.com'},
        ]
```

Now, the Event object looks like this:

```javascript
const event = {
    summary: 'Tech Talk with Arindam',
    location: 'Google Meet',

    description: "Demo event for Arindam's Blog Post.",
    start: {
        dateTime: "2024-03-14T19:30:00+05:30",
        timeZone: 'Asia/Kolkata'
    },
    end: {
        dateTime: "2024-03-14T20:30:00+05:30",
        timeZone: 'Asia/Kolkata'
    },
    colorId: 1,
    conferenceData: {
        createRequest: {
            requestId: uuid(),
        }
    },

    attendees: [
        {email: 'arindammajumder2020@gmail.com'},
    ]

};
```

Next, In the event insertion step, We'll add `conferenceDataVersion` parameter to it.

```javascript
const result = await calendar.events.insert({
                    calendarId: 'primary', 
                    auth:oauth2Client , 
                    conferenceDataVersion: 1, 
                    resource: event
               });
```

This will create an Event with a Google Meet Link. We can also share the link as a response like this:

```javascript
        res.send({
            status: 200,
            message: 'Event created',
            link: result.data.hangoutLink
        });
```

### Sending Reminder to the Attendees:

So we're almost done with our project, just the final touch is remaining. Till now, Our project will directly add the event to the calendar of the invited guests.

However, to notify them about these events, we have to send one Mail. For that, we have to add one parameter `sendUpdates: 'all',` in the event creation part. With this, the application will automatically send emails to the invited guests.

```javascript
const result = await calendar.events.insert({
                    calendarId: 'primary', 
                    auth:oauth2Client , 
                    conferenceDataVersion: 1 , 
                    sendUpdates: 'all', 
                    resource: event
              });
```

With this addition, our project now seamlessly handles both event creation and email notifications.

## Testing the Application:

The Coding part of our Project is Completed. Now, let's see if it's working or not!

For that, we'll start the project!

```javascript
npm run start
```

And We have started our Server on port 8000!  
Now, we'll Go to the [http://localhost:8000/auth/](http://localhost:8000/auth/) Route to Authenticate the user. It will redirect us to something like this:

![Sign in Page ](https://cdn.hashnode.com/res/hashnode/image/upload/v1709444909539/0b838408-d31f-4a1b-8913-f12944b4cb28.png align="center")

It will ask for some permission for the application

![Premission page](https://cdn.hashnode.com/res/hashnode/image/upload/v1710304258394/5564b28b-9c95-407f-a8fe-5c6109244721.png align="center")

It will redirect to `/auth/redirect` route with the `code` query parameter.

![/auth/redirect route](https://cdn.hashnode.com/res/hashnode/image/upload/v1710304314219/89f9c434-214e-4b4c-9a99-bd80835a6914.png align="center")

After successfully authenticating the user, We'll go to the [http://localhost:8000/create-event](http://localhost:8000/create-event) route to schedule the Event.

![http://localhost:8000/create-event route](https://cdn.hashnode.com/res/hashnode/image/upload/v1710305177320/4708acc5-0ce6-47d9-8fe2-f5bbda789e89.png align="center")

Awesome! It means our Event is created with a Google Meet link.

To verify that the event creation process is functioning correctly, Let's check our Google Calendar

![Google Calendar ](https://cdn.hashnode.com/res/hashnode/image/upload/v1710305774546/4204922a-b5c4-4d74-a19e-6a05043e4e0d.png align="center")

Amazing! Our Event is Created which means the Event-creation route is working perfectly! And We also got an Invite mail as well:

![Event Invite Mail.](https://cdn.hashnode.com/res/hashnode/image/upload/v1710305452853/5a405e55-1590-4cc7-b1cb-263c91fd1679.png align="center")

Great! Our Application is working perfectly!

With that, We have integrated Google Calendar into our Node.js app. In the following articles, We'll explore more use cases with Google Calendar.

Till then, Stay Tuned!

## Conclusion

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web Development topics.

For Paid collaboration, mail me at: [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729), and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![Thank You](https://cdn.hashnode.com/res/hashnode/image/upload/v1710281561678/3d027d62-650e-4197-b2b2-1b92738f0641.png align="center")