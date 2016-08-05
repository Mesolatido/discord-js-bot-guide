# Understanding Resolvables
In this page we'll explore the different ways in which you can use resolvables, and identify them.

In the discord.js documentation you will come across the term *resolvable*. In essence, this word means "something used to identify an object". For instance, a *User Resolvable* is, "something used to identify a user". 


Take for instance the *Channel Resolvable*. One of its main uses is inside the **client.sendMessage()** function, where it defines the, you guessed it, *Channel* where the message is sent. Here's what the documentation says are acceptable channel resolvables: Channel, Server, Message, User (in some instances), String of Channel ID, String of User ID.

Let's pretend you're within your `message` event handler, where you've received a message from a user on a channel. You want to reply with a message, not to the channel, but directly to the user. **client.sendMessage()** indicates that a *Channel Resolvable* can be used in its first parameter. Using the list above, then, we know that `User` is accepted. In this context, it means the message is sent to the user, *in a private message*. 

But how do you get the user? There are 2 user resolvables in the list: `User` and `String of User ID`. `User` is a user object, meaning something like `message.author` is acceptable. But it also means that a direct ID such as, say, "139412744439988224" can also work. 

## Getting resolvables

Clearly, using a resolvable in context is straightforward. `message.server` is a server resolvable, `message.author.id` is a user resolvable, and just `message` is a message resolvable (all in the context of a message of course). 

But what if you don't have the object or even the ID available? One of the questions that come back often on #node_discord-js is the use of *Role Resolvables*. *Roles* are often used to check if a user has permission to access a certain command, and the biggest mistake new users make is to try this: 

`if(message.server.memberHasRole("Bot Commander")) { do something; }`

Unfortunately, the *name* of a role is not considered a *role resolvable* and this fails. This is where caches come in: 

`if(message.server.memberHasRole(message.server.roles.get("name", "Bot Commander"))) { works;}`

