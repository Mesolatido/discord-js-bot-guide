# Getting Started - Linux Short Version

> **This is the TL;DR version for Linux**. If you wish for a long version with more explanations, please see [this guide](the-long-version.html)

## Create App and Bot Account

 - Go to the [Discordapp.com Application Page](https://discordapp.com/developers/applications/me)
 - Create a **New Application**, and give it a name
 - Click **Create a bot account**, then **Yes, do it**
 - Visit https://discordapp.com/oauth2/authorize?client_id=APP_ID&scope=bot , replacing **APP_ID** with the **Client/Application ID** from the app page, to add the bot to your server (or ask a server admin to do it for you).
 - Copy your bot's **Secret Token** and keep it for later

## Pre-requisite software

Install the following through your package manager: 

 - nodejs (Version 6.X and higher required, see [here](https://nodejs.org/en/download/package-manager/))

Once you have this all installed, create a folder for your project and install discord.js: 

`mkdir mybot && cd mybot`
`npm install discord.js`

**For sound support** add `npm install opusscript` (ez mode) or `npm install node-opus` (better performance but requires `python 2.7.x` and `build-essential`). BOTH these options require `ffmpeg` to run on your system, installed through `sudo apt-get install ffmpeg`.

## Example Code

The following is a simple ping/pong bot. Save as a text file (e.g. `mybot.js`), replacing the string on the last line with the secret bot token you got earlier: 

```js
var Discord = require("discord.js");
var bot = new Discord.Client();

bot.on("message", msg => {
	if (msg.content.startsWith("ping")) {
		msg.channel.sendMessage("pong!");
	}
});

bot.on('ready', () => {
  console.log('I am ready!');
});

bot.login("yourcomplicatedBotTokenhere");
```

## Launching the bot

In your terminal, from inside the folder where `mybot.js` is located, launch it with: 

`node mybot.js`	

If no errors are shown, the bot should join the server(s) you added it to.

## Resources

 - [Discord.js Examples](https://github.com/hydrabolt/discord.js/tree/master/docs/custom/examples) : Structured, useful examples of bots with one or more commands.
 - [Discord.js Documentation](http://discord.js.org/) : Full API reference for discord.js
 - [Discord.JS Official Server](https://discord.gg/seraph-leblanc-oracle): Join us on #node_discord-js