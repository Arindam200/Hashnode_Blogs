---
title: "Create a Discord bot with NodeJS"
seoDescription: "Learn how to create a Discord bot with NodeJS, add functionalities, and automate tasks for your server"
datePublished: Sat Mar 09 2024 15:41:22 GMT+0000 (Coordinated Universal Time)
cuid: cltk96mib000a09jvb2a18u85
slug: create-a-discord-bot-with-nodejs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702240178677/533d00c4-cc4e-44b0-b293-cbfa62b91b66.png
tags: javascript, web-development, nodejs, beginners, discord, discord-bot, chaicode

---

## Introduction

We, developers always love to play with APIs to automate our boring activities. And With Discords API we can automate those stuff using Discord bots.

In this article, we'll explore How to create a Discord bot using NodeJs and also Add a few functionalities to it!

Sounds Interesting?

So, Without delaying Further, Let's START!

![Lets Go Let Start GIF - LetsGo LetStart LetsGetItOn GIFs](https://imgs.search.brave.com/XP0T3bOxpfAaSh1n0AMF1byWzJoA-QXGgZPWGDpOMFc/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9tZWRp/YTEudGVub3IuY29t/L2ltYWdlcy82MmIy/YzFiZTMyYWMzNjZj/NTJjYzljNTdkMzU1/NjAyZi90ZW5vci5n/aWY_aXRlbWlkPTEy/MzUyNTM5.gif align="center")

## What is Discord?

![GIF of Discord logo](https://assets-global.website-files.com/5f9072399b2640f14d6a2bf4/6116dabb8ea9fc566fc37b7e_0*MrMnYKtKDKRcZsmS.gif align="left")

[Discord](https://discord.com/) is a popular instant messaging application. Originally designed for gamers, it has evolved into a versatile space for communities to connect and collaborate in real-time.

If you’re familiar with Discord, you may have noticed the presence of a Bot. Discord Bots can perform various actions such as sending messages to servers, DM-ing users, moderating servers, and playing audio in voice chats.

## Setting up a Discord bot:

In this section, We'll be setting up our Discord bot! We will use Discord developer's graphical user interface (GUI) to set up a Discord bot.

So First, Let's go to the [Discord Application Dashboard](https://discord.com/developers/applications/). Here we'll see the following interface!

![Discord Applications Dashboard](https://cdn.hashnode.com/res/hashnode/image/upload/v1702100799706/7130d534-0557-4490-b89c-13158227ad0d.png align="center")

To get started, We will click the `New Application` button and then Discord will ask us to enter a name for our new application.

For this Article, I'm taking the name as `Arindam_testBot` . After that click the `Create` button!

![Create an Application interface](https://cdn.hashnode.com/res/hashnode/image/upload/v1702100885599/de2cbcf6-1dfb-49d7-baba-129e5ada19c9.png align="center")

Before Moving further We have to allow one setting in our Discord. We will turn on the Developer Mode so that we can create Discord bots.

![Image of the Discord application dashboard after first visiting https://discord.com/developers/applications](https://cdn.hashnode.com/res/hashnode/image/upload/v1702102223582/2471006a-93bd-4e1c-95fe-ccaf4049a534.png align="center")

Next, We'll add the bot to the server. So, we'll create an invite to add the bot to a Discord guild/server.

We'll get the **URL Generator** page in the **OAuth2** tab. To create the link, we have to select the bot option under the scopes and we'll get the link.

![OAuth2 tab, with scope set to "bot" and permissions set to "administator"](https://cdn.hashnode.com/res/hashnode/image/upload/v1702102921415/9d46a583-6789-4c55-8724-b0931d110c4e.png align="center")

Also, we can decide which permission we are going to give to the bot. For this example, I'm giving the Administrator permission to it.

![OAuth2 tab permissions set to "administator"](https://cdn.hashnode.com/res/hashnode/image/upload/v1702102952235/2690155e-8f5a-4d1d-b41a-c46aad52fbc5.png align="center")

Then It will generate a URL like this : `https://discord.com/api/oauth2/authorize?client_id=11216324252454&permissions=8&scope=bot`

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">This is an example link. Don't copy this as it won't work for you. Copy the link that you will get from the Dashboard</div>
</div>

After Going to that Link We'll have to select the server where we want to add the Bot to. In this case, We'll add that to the newly created server.

![Page from following the invite link, allowing users to add the bot to servers](https://cdn.hashnode.com/res/hashnode/image/upload/v1702103092568/009b0b90-07a1-4afd-a8b2-45118142d6dc.png align="center")

![Discord message letting us know a bot has joined the server](https://cdn.hashnode.com/res/hashnode/image/upload/v1702103266770/1b701c81-cb68-454e-90a4-82d0993850a4.png align="center")

And That's it! We have set up our bot and added it to our Server!

Now, Let's move to the Project Setup part!

## Project Setup:

In this section, we'll set up a basic coding environment for our Project.

To kick things off, we'll initialize a Node.js project.

```javascript
npm init
```

This will create a `package.json` file like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702241133913/c7b04ebf-37b1-4bf3-8218-578032482473.png align="center")

Now, We'll create a file named `config.json` to store our bot’s authentication token. Then add the following code to the config file.

```javascript
//config.json
{
    "BOT_TOKEN": "YOUR BOT TOKEN"
}
```

> Replace the YOUR BOT TOKEN with your token that we have generated before.

To interact with the bot we'll use a package called `Discord.js`.We'll install `discord.js` through npm with the following command:

```javascript
npm install discord.js
```

Now we have set up the configuration file and installed the basic dependencies. Let's move to the next step!

So, To work with the Discord bot we need to log in as a Discord bot! For that, we'll be taking the bot token and adding that to the config.json file.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Note: Never share your Bot token publicly. With that token, anyone can use your bot!</div>
</div>

Now we'll start coding the Discord bot using Discord.js!

In the `index.js` file At first, we'll require the Discord package that we have just installed and Create a new client.

```javascript
const { Client, GatewayIntentBits } = require("discord.js");
const client = new Client({
  intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildMessages,
    GatewayIntentBits.MessageContent,
  ],
});
```

Here We have created our virtual client with which we'll interact with our server. The intents represent the permissions that we are giving to it. To know more about Gateway intent check this [doc](https://discord.com/developers/docs/topics/gateway#gateway-intents).

Also, We have to turn on this setting to give the bot access to send messages to the server.

![setting to give the bot access to send messages to the server.](https://cdn.hashnode.com/res/hashnode/image/upload/v1702242617736/183e3f6b-c0f5-4a6c-abc3-5e4b59bf6396.png align="center")

So, Our bot has access to the guilds, writing messages, and reading the content of the messages!

```javascript
client.on("messageCreate", function (message) {
  console.log(message.content);
});
```

Here, When a message is created this callback function will trigger and will log the `message.content`.

We also have to log in using the Token we kept in the `config.json` file.

```javascript
const config = require("./config.json");

client.login(config.BOT_TOKEN);
```

So, Let's check how it works by starting our server! We'll write a message and see if it logs the message or not!

![Written First message in Discord server](https://cdn.hashnode.com/res/hashnode/image/upload/v1702244425030/e20f0c4f-1346-4ba2-baa3-7b3d3a460037.png align="center")

Now, Let's see the Terminal!

![Terminal image that prints Hello I'm Arindam](https://cdn.hashnode.com/res/hashnode/image/upload/v1702244457359/a97069c3-002e-4b7f-acc6-f90eb106f2f8.png align="center")

Oh Wow! It's Working!! So we have created our very basic discord bot functionality!

In the next section, we'll see some advanced functionality implementations!

## Adding Functionality:

In the previous section, we created a basic Discord bot! Now, It's time to add some functionalities to it!

Excited??

\- Me Too!!

![I Am So Excited Ryan Bruce Sticker - I Am So Excited Ryan Bruce Fluff Stickers](https://imgs.search.brave.com/wrlyxjyjuMOAJmF3SyLxHLlHZTZ1W99YRnS6RT_ohFY/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9tZWRp/YS50ZW5vci5jb20v/RkJ3SV90b1A5TEVB/QUFBai9pLWFtLXNv/LWV4Y2l0ZWQtcnlh/bi1icnVjZS5naWY.gif align="left")

### Replying to a Message:

Now instead of just logging the message in the console, we'll reply to a message!

So let's update the code to this:

```javascript
client.on("messageCreate", function (message) {
  message.reply({ content: "Hello From Bot" });
});
```

Let's start the server again and see what happens!

![Discord Chat Screen Shot](https://cdn.hashnode.com/res/hashnode/image/upload/v1702245525404/c5fd2c17-2501-4fd9-946a-8d090b0b6a6a.png align="center")

It works! But there's a problem! It replies to its own messages and creates an infinite loop!

To Stop that, we have to add one check to that! The updated code will be:

```javascript
client.on("messageCreate", function (message) {
  if (message.author.bot) return;
  message.reply({ content: "Hello From Bot" });
});
```

This line checks if the author of the message is a bot, and if so, stops processing the command. Let's see that:

![Discord chat Screen Shot](https://cdn.hashnode.com/res/hashnode/image/upload/v1702245883044/535fe457-4130-44fd-8f0a-5da1c8a42a89.png align="center")

Okay So Now, it's working perfectly! That's Great Let's add some more functionalities!

### Creating Slash Commands:

Now we'll create a slash command. It is pretty straightforward.

Here, we'll import Rest, Routes from the `discord.js` and create a Rest client using the Token we have created before.

```javascript
const { REST, Routes } = require("discord.js");
const config = require("./config.json");

const rest = new REST({ version: "10" }).setToken(config.BOT_TOKEN);
```

Now we will create our command and handle it.

```javascript
const commands = [
  {
    name: 'describe',
    description: 'Gives info about Arindam',
  },
];

try {
  console.log("Started refreshing application (/) commands.");

  await rest.put(Routes.applicationCommands(CLIENT_ID), { body: commands });

  console.log("Successfully reloaded application (/) commands.");
} catch (error) {
  console.error(error);
}
```

> We will get the Client Id from the OAuth

So Basically this function will register our newly created command.

Let's start our server and see if it's registered or not!

![Discord command Screenshot](https://cdn.hashnode.com/res/hashnode/image/upload/v1702762315608/78752c89-414b-49ae-93fd-4e453d2ae908.png align="center")

It's Working! Great! So we have registered our command.

Now we'll add functionalities to it.

```javascript
client.on('interactionCreate', async interaction => {
  if (!interaction.isChatInputCommand()) return;

  if (interaction.commandName === 'describe') {
    await interaction.reply('Arindam is a Technical Writer');
  }
});
```

Normal messages are handled by `"messageCreate"` and the interactions/ Slash commands are handled by `'interactionCreate'`.

This will reply back with 'Arindam is a Technical Writer' when we will trigger the command.

So, Let's see if it's really working or not!

![Discord command working ](https://cdn.hashnode.com/res/hashnode/image/upload/v1702763540786/f8969836-0c8d-46ea-8252-ca192072717d.png align="center")

And Yesss...It's Working!

Now, With this, You can also create your own customised Discord Bot!

For Reference, You can check the Code Here:

%[https://github.com/Arindam200/DiscordBot] 

## Conclusion:

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web Development topics.

For Paid collaboration mail me at : [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![Thank you Image](https://cdn.hashnode.com/res/hashnode/image/upload/v1702157445390/c6e1965c-51d3-45d0-bda6-73f76d9b2253.png align="center")