# Getting Started - Linux Short Version

> **This is the TL;DR version for Linux**. If you wish for a long version with more explanations, please see [this guide]()

## Create App and Bot Account

 - Go to the [Discordapp.com Application Page](https://discordapp.com/developers/applications/me)
 - Create a **New Application**, and give it a name
 - Click **Create a bot account**, then **Yes, do it**
 - Visit https://discordapp.com/oauth2/authorize?client_id=APP_ID&scope=bot , replacing **APP_ID** with the **Client/Application ID** from the app page, to add the bot to your server (or ask a server admin to do it for you).
 - Copy your bot's **Secret Token** and keep it for later

## Pre-requisite software

Install the following through your package manager: 

 - nodejs (version 4 or 6 recommended, see [here](https://nodejs.org/en/download/package-manager/))
 - python 2.7.x

If you need sound support, add the following: 

- ffmpeg
- build-essential

Once you have this all installed, create a folder for your project and install discord.js: 

`mkdir mybot && cd mybot`
`npm install discord.js`

## Example Code

The following is a simple ping/pong bot. Save as a text file (e.g. `mybot.js`), replacing the string on the last line with the secret bot token you got earlier: 

```js
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

In your terminal, from inside the folder where `mybot.js` is located, launch it with: 

`node mybot.js`	

If no errors are shown, the bot should join the server(s) you added it to.

## Resources

 - [Discord.js Examples](https://github.com/hydrabolt/discord.js/tree/master/examples) : Structured, useful examples of bots with one or more commands.
 - [Discord.js Documentation](http://discordjs.readthedocs.io/en/latest/index.html) : Full API reference for discord.js
 - [Discord API server](https://discord.gg/seraph-leblanc-oracle): Join us on #node_discord-js