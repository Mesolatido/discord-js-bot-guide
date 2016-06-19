# Understanding Events and Handlers

We already explored one event handler in [Your Basic Bot](your-basic-bot.html), the `message` handler. Now let's take a look at some of the most important handlers that you will use, along with an example.

> **Don't nest events**
> One important point: Do not nest any events within others unless you know what you're doing. Events should be at the "root" level of your code, *beside* the `message` handler and not within it. 

## The `ready` event and its importance

Ah, asynchronous coding. So awesome. So hard to grasp when you first encounter it. The reality of discord.js and many, many other libraries you will encounter, is that code is not executed one line at a time, one after the other. 

It should have been made obvious with the user of `bot.on("message")` which triggers for each message. To explain how the `ready` event is important, let's look at the following code: 

```js
var Discord = require("discord.js");
var mybot = new Discord.Client();

console.log(mybot.user.id);

mybot.loginWithToken("token");
```

This code will not work, because `mybot` is not immediately available after it's been initialized. `mybot.user` will be undefined in this case, even if we flipped the console.log and login lines. Well it might work - sometimes. This is because it takes a small amount of time for discord.js to load its servers, users, channels, and all that jazz. The more servers the bot is on, the longer it takes. 

To ensure that `bot` and all its "stuff" is ready, we can use the `ready` event. Any code that you want to run on bootup that requires access to the `bot` object, will need to be in this event.

Here's a simple example of using the `ready` event handler:

```js
bot.on("ready", () => {
	console.log(`Ready to server in ${bot.channels.length} channels on ${bot.servers.length} servers, for a total of ${bot.users.length} users.`);
});
```

I have this in all my bots, in various forms. If you need to loop across all your servers, this is also where you would do it.


## Detecting New Members

Another useful event is `serverNewMember` which triggers whenever someone joins any of the servers the bot is on. You'll see this on smaller servers: a bot welcomes every new member in the #general channel. The following code does this.

```js
bot.on("serverNewMember", (server, user) => {
	console.log(`New User "${user.username}" has joined "${server.name}"` );
	bot.sendMessage(server.defaultChannel, `"${user.username}" has joined this server`);
});
```





## Errors, Warn and Debug messages

Yes, bots fail sometimes. And yes, the library can too! Even though this might be 