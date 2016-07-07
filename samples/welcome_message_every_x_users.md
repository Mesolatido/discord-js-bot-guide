# Welcome Message every X users

This sample will show how to keep an array/object of new users coming into a server. Then, when this array reaches a certain number of users, it shows a message welcoming those users as a group. When your server becomes popular and you get dozens of users every day, you will find this to be much less annoying than welcoming one user at a time!

The events we're going to use in this example: 
* `serverNewMember` , which triggers when a new user joins the server.
* `serverMemberRemoved` , which triggers when a user leaves the server.

Also, we're going to be using a `Discord.Cache()` object to save the users. Why? Because it's available, has great helper methods, and is *built* to support the Discord.js objects we're putting in it!

Initializing the discord cache is simple: `var newUsers = new Discord.Cache();` . This has to be done *outside* of the events we're going to use. I have it at the top of my file, *after* the `var Discord = require("discord.js")` line, obviously.

Adding new members that join to the cache is simple:

```js
bot.on("serverNewMember", (server, user) => {
  newUsers.add(user);
});
```

If a user leaves while he's on that list though, it would cause your bot to welcome @invalid-user. To fix this, we remove that user from the cache: 

```js
bot.on("serverMemberRemoved", (server, user) => {
  if(newUsers.has(user)) newUsers.remove(user);
});
```

But wait, where do we welcome users? That's done in `serverNewMember`, when the count reaches the number you want: 

```js
bot.on("serverNewMember", (server, user) => {
  newUsers.add(user);

  if(newUsers.length > 10) {
    var userlist = newUsers.map(u => u.mention()).join(" ");
    bot.sendMessage(server.defaultChannel, "Welcome our new users!\n"+userlist);
    newUsers = new Discord.Cache();
  }
});
```

Two lines require a little more explanation: 
* `newUsers.map(u => u.mention()).join(" ");` uses the fancy ES6 `map` function to get a mention for each user in the array, then joins them with a space between each.
* `newUsers = new Discord.Cache();` *resets* empties the cache completely, so it resets to 0.

## Multiple servers?

The only issue with the above code is that it would only work if your bot is on a single server. Though this might be alright you, there's a chance you want to support multiple servers. How do we do that? We change `newUsers` to an `Array` instead, and each server gets its own cache. Here is a **complete** example that does nothing but welcome new users: 

```js
'use strict'; // enables ES6 syntax

const Discord = require('discord.js');
const bot = new Discord.Client({autoReconnect: true});

const newUsers = [];

bot.on("serverNewMember", (server, user) => {
  if(!newUsers[server.id]) newUsers[server.id] = new Discord.Cache();
  newUsers[server.id].add(user);

  if(newUsers[server.id].length > 10) {
    var userlist = newUsers[server.id].map(u => u.mention()).join(" ");
    bot.sendMessage(server.defaultChannel, "Welcome our new users!\n"+userlist);
    newUsers[server.id] = new Discord.Cache();
  }

});



bot.loginWithToken("Your.Token");
```