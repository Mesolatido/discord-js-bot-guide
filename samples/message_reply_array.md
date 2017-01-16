# Message Reply Array

> **PLEASE NOTE THIS GUIDE WILL NO LONGER BE UPDATED DUE TO DRAMA**

This sample shows the use of a simple string array to reply specific strings when triggered. 

I have often seen the following type of code happen in new bots: 

```js
bot.on('message', (message) => {
  if(message.content === "ayy") {
    message.channel.sendMessage("Ayy, lmao!");
  }
  if(message.content === "wat") {
    message.channel.sendMessage("Say what?");
  }
  if(message.content === "lol") {
    message.channel.sendMessage("roflmaotntpmp");
  }
  // Imagine 20, 50, 70 almost identical conditions.
}
```

Ignore the fact that this code doesn't have a prefix and also does not ignore itself or other bots for now. The important fact here is that we can reduce this to a much simpler code, through the use of a array. Well, actually, to make things simpler, let's use an object instead.

First, we declare this object: 

```js
var responseObject = {
  "ayy": "Ayy, lmao!",
  "wat": "Say what?",
  "lol": "roflmaotntpmp"
};
```

This simple object (which can easily be in a JSON file) can then be used in a single command checker, which would look like this: 

```js
bot.on('message', (message) => {
  if(responseObject[message.content]) {
    message.channel.sendMessage(responseObject[message.content]);
  }
});
```

That code basically says: "If you find the message content to be a key of the responseObject, send a message containing that key's value". 

Boom. Done.