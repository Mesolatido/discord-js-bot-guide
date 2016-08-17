# Command with arguments

In [Your First Bot](../coding-walkthroughs/your_basic_bot.md), we explored how to make more than one command. These commands all started with a prefix, but didn't have any *arguments* : extra parameters used to vary what the command actually does.

## Creating an array of arguments

The first thing that we need to do to use arguments, is to actually separate them. A command with arguments would normally look something like this: 
`!mycommand arg1 arg2 arg3`

In [Your First Bot](../coding-walkthroughs/your_basic_bot.md) we actually simplify our task just a bit: our check for commands uses `startsWith()`: 

```js
if (msg.content.startsWith(prefix + "ping")) {
  bot.sendMessage(msg, "pong!");
} 
```

This means that `!ping whaddup` or `!ping I'm a little teapot` would both trigger the command, and the bot would respond "pong!". It would just ignore everything after the command because of course we're not doing anything with it.

Let's start with creating an *Array* containing each word after our command, using the `.split()` function: 

```js
if (msg.content.startsWith(prefix + "ping")) {
  let args = msg.content.split(" ");
} 
```

This generates an array that would look like `["!ping", "I'm", "a", "little", "teapot"]`, for instance. We can then access any part of that array using `args[0]` to `args[4]`, which return the string in that array position. And if you want to be more precise and don't want `args` to contain the `command`, you can just remove it from the array: `let args = msg.content.split(" ").slice(1);`. 

If you want, you can then specify argument *names* by referring to the array positions: 

```js
if (msg.content.startsWith(prefix + "asl")) {
  let args = msg.content.split(" ").slice(1);
  let age = args[0]; // yes, arrays are 0-based. I hate that too.
  let sex = args[1]; // if you laugh at this variable name, grow up.
  let location = args[2];
  bot.reply(msg, `Hello ${msg.author.name}, I see you're a ${age} year old ${sex} from ${location}. Wanna date?`);
} 
```

And if you want to be **really** fancy with ECMAScript 6, here's an awesome one (requires Node 6!):
```js
if (msg.content.startsWith(prefix + "asl")) {
  let [age, sex, location] = msg.content.split(" ").slice(1);
  bot.reply(msg, `Hello ${msg.author.name}, I see you're a ${age} year old ${sex} from ${location}. Wanna date?`);
} 
```

> This is called [Destructuring](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) and it's awesome! 

## Grabbing Mentions

Another way to use arguments, when the command should target a specific user (or users), is to use *Mentions*. For instance, to kick annoying shitposters with `!kick @Xx_SniperBitch_xX @UselessIdiot` can be done with ease, instead of attempting to grab their ID or their name.

In the context of the `message` event handler, all mentions in a message are part of the `msg.mentions` array. Each value in the array is a full `user` resolvable so you can get their ID, name, etc. 

Let's build a quick and dirty `kick` command, then.

```js
// Kick a single user in the mention
if (msg.content.startsWith(prefix + "kick")) {
  // I'll make a code example on how to check if the user is allowed, one day!
  let userToKick = msg.mentions[0];
  bot.kickMember(userToKick, msg.server).catch( (e) => { if(e) console.error(e) });
  // see I even catch the error!
} 
```

If you want to support *multiple* mentions, then you'll have to loop through each of the mentions. I mean, you *should* know how to loop, but I'm feeling generous with spoonfeeding today: 

```js
if (msg.content.startsWith(prefix + "kick")) {
  // I'll make a code example on how to check if the user is allowed, one day!
  msg.mentions.map( (user) => {
    bot.kickMember(user, msg.server).catch( (e) => { if(e) console.error(e) });
  });
  // Yes that's right, I'm using ECMAScript 6 to confuse you again.
} 
```

> There's a reason I'm adding error catching here. It's because the first time you use this command, you are most likely going to come to the realization that your bot does not have kick permissions. That is, assuming you actually read error messages and understand the word "Forbidden".

## Variable Length arguments

Let's say you want to have a command that creates a new role. To be honest I wouldn't - roles should be created manually - but for the sake of argument let's do it.

We'll give the role command 3 arguments: A role *name*, a role *color* and whether the role is *hoisted* (aka, shows separately in the user list). The order of these arguments is important here: *hoisted* and *color* will always be one word, but the role *name* can be variable. "Bot Commander" "Admin People", "Eternal Noobs" are all valid role names. So let's handle them right!

(((( SHALL CONTINUE IN A MOMENT  PLEASE STAND BY ))))