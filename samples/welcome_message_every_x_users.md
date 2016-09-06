# Welcome Message every X users

This sample will show how to keep an array/object of new users coming into a server. Then, when this array reaches a certain number of users, it shows a message welcoming those users as a group. When your server becomes popular and you get dozens of users every day, you will find this to be much less annoying than welcoming one user at a time!

The events we're going to use in this example: 
* `guildMemberAdd` , which triggers when a new user joins the server.
* `guildMemberRemove` , which triggers when a user leaves the server.

Also, we're going to be using a `Discord.Collection()` object to save the users. Why? Because it's available, has great helper methods, and is *built* to support the Discord.js objects we're putting in it!

Initializing the discord collection is simple: `var newUsers = new Discord.Collection();` . This has to be done *outside* of the events we're going to use. I have it at the top of my file, *after* the `const Discord = require("discord.js")` line, obviously.

Adding new members that join to the collection is simple:

```js
bot.on("guildMemberAdd", (guild, user) => {
  newUsers.set(user.id, user);
});
```

If a user leaves while he's on that list though, it would cause your bot to welcome @invalid-user. To fix this, we remove that user from the collection: 

```js
bot.on("guildMemberRemove", (guild, user) => {
  if(newUsers.exists("id", user.id)) newUsers.delete(user.id);
});
```

But wait, where do we welcome users? That's done in `serverNewMember`, when the count reaches the number you want: 

```js
bot.on("guildMemberAdd", (guild, user) => {
  newUsers.set(user.id, user);

  if(newUsers.size > 10) {
    var userlist = newUsers.map(u => u.mention()).join(" ");
    guild.channels.get(guild.id).sendMessage("Welcome our new users!\n"+userlist);
    newUsers = new Discord.Collection();
  }
});
```

Two lines require a little more explanation: 
* `newUsers.map(u => u.mention()).join(" ");` uses the fancy ES6 `map` function to get a mention for each user in the array, then joins them with a space between each.
* `newUsers = new Discord.Collection();` *resets* empties the cache completely, so it resets to 0.

## Multiple servers?

The only issue with the above code is that it would only work if your bot is on a single server. Though this might be alright you, there's a chance you want to support multiple servers. How do we do that? We change `newUsers` to an `Array` instead, and each server gets its own cache. Here is a **complete** example that does nothing but welcome new users: 

```js
'use strict'; // enables ES6 syntax

const Discord = require('discord.js');
const bot = new Discord.Client({autoReconnect: true});

const newUsers = [];

bot.on("guildMemberAdd", (guild, user) => {
  if(!newUsers[guild.id]) newUsers[guild.id] = new Discord.Collection();
  newUsers[guild.id].add(user);

  if(newUsers[guild.id].size > 10) {
    var userlist = newUsers[guild.id].map(u => u.mention()).join(" ");
    guild.channels.get(guild.id).sendMessage("Welcome our new users!\n"+userlist);
    newUsers[guild.id] = new Discord.Collection();
  }

bot.on("guildMemberRemove", (guild, user) => {
  if(newUsers[guild.id].exists("id", user.id)) newUsers.delete(user.id);
});

});

bot.loginWithToken("Your.Token");
```