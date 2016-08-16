# Getting Started with Discord.js

> **This is the TL;DR version for Windows**. If you wish for a long version with more explanations, please see [this guide](the-long-version.html)

## Create App and Bot Account

 - Go to the [Discordapp.com Application Page](https://discordapp.com/developers/applications/me)
 - Create a **New Application**, and give it a name
 - Click **Create a bot account**, then **Yes, do it**
 - Visit https://discordapp.com/oauth2/authorize?client_id=APP_ID&scope=bot , replacing **APP_ID** with the **Client/Application ID** from the app page, to add the bot to your server (or ask a server admin to do it for you).
 - Copy your bot's **Secret Token** and keep it for later

## Pre-requisite software

Install the following software in Windows: 

 - nodejs from [the downloads page](https://nodejs.org/en/download/)

If you need sound support, you'll need 2 more things: 

- ffmpeg which is available [on this page](http://adaptivesamples.com/how-to-install-ffmpeg-on-windows/)
- The new windows build tools:
  - Open an ADMIN command prompt, or PowerShell
  - Run the following command: `npm i -g --production windows-build-tools`
  - This installs Python 2.7 and the C++ Build Tools standalone.

Once you have this all installed, create a folder for your project and install discord.js: 

`md mybot`
`cd mybot`
`npm install discord.js`

## Example Code

The following is a simple ping/pong bot. Save as a text file (e.g. `mybot.js`), replacing the string on the last line with the secret bot token you got earlier: 

```js
'use strict';
var Discord = require("discord.js");
var bot = new Discord.Client();

bot.on("message", msg => {
	if (msg.content.startsWith("ping")) {
		bot.sendMessage(msg, "pong!");
	}
});

bot.loginWithToken("yourcomplicatedsecretcodehere");
```

## Launching the bot

In your command prompt, from inside the folder where `mybot.js` is located, launch it with: 

`node mybot.js`	

If no errors are shown, the bot should join the server(s) you added it to.

## Resources

 - [Discord.js Examples](https://github.com/hydrabolt/discord.js/tree/master/examples) : Structured, useful examples of bots with one or more commands.
 - [Discord.js Documentation](http://discordjs.readthedocs.io/en/latest/index.html) : Full API reference for discord.js
 - [Discord API server](https://discord.gg/seraph-leblanc-oracle): Join us on #node_discord-js