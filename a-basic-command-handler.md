# A Basic Command Handler Example```

> This was *supposed* to be covered in my next YouTube video. But, the footage was unuseable and I'm no longer making videos, so all you get is this code.

A *Command Handler* is essentially a way to separate your commands into different files, instead of having a bunch of `if/else` conditions inside your code (or a `switch/case` if you're being fancy).

In this case, the code shows you how to separate each command into its own file. This means that each command can be *edited* separately, and also *reloaded* without the need to restart your bot. Yes, really!

## App.js Changes

Without going into all the details of the changes made (which would have been in the video), here is the modified app.js file:

```js
const Discord = require("discord.js");
const bot = new Discord.Client();
const fs = require("fs");

const config = require("./config.json");

// This loop reads the /events/ folder and attaches each event file to the appropriate event.
fs.readdir("./events/", (err, files) => {
  if(err) return console.error(err);
  files.forEach(file => {
    let eventFunction = require(`./events/${file}`);
    let eventName = file.split(".")[0];
    // super-secret recipe to call events with all their proper arguments *after* the `bot` var.
    bot.on(eventName, (...args) => eventFunction.run(bot, ...args));
  });
});

bot.on("message", message => {
  if (message.author.bot) return;
  if (!message.content.startsWith(config.prefix)) return;

  let command = message.content.split(" ")[0];
  command = command.slice(config.prefix.length);

  let args = message.content.split(" ").slice(1);

  // The list of if/else is replaced with those simple 2 lines:
  let commandFile = require(`./commands/${command}.js`);
  commandFile.run(bot, message, args);
});

bot.login(config.token);
```

## Example commands

This would be the content of the `./commands/ping.js` file, which is called with `!ping` (assuming `!` as a prefix)

```js
exports.run = (bot, message, args) => {
    message.channel.sendMessage("pong!").catch(console.error);
}
```

Another example would be the more complex `./commands/kick.js` command, called using `!kick @user`

```js
exports.run = (bot, message, args) => {
  let modRole = message.guild.roles.find("name", "Mods");
  if(!message.member.roles.has(modRole.id)) {
    return message.reply("You pleb, you don't have the permission to use this command.").catch(console.error);
  }
  if(message.mentions.users.size === 0) {
    return message.reply("Please mention a user to kick").catch(console.error);
  }
  let kickMember = message.guild.member(message.mentions.users.first());
  if(!kickMember) {
    return message.reply("That user does not seem valid");
  }
  if(!message.guild.member(bot.user).hasPermission("KICK_MEMBERS")) {
    return message.reply("I don't have the permissions (KICK_MEMBER) to do this.").catch(console.error);
  }
  kickMember.kick().then(member => {
    message.reply(`${member.user.username} was succesfully kicked.`).catch(console.error);
  }).catch(console.error)
}
```

Notice the structure on the first line. `exports.run` is the "function name" that is exported, with 3 arguments: `bot` (the client), `message` (the message variable from the handler) and `args` (the array of arguments). The actual code is not changed in any way, it's a simple copy/paste into the file itself. Everything works the same. 

## Example Events

Events are handled almost exactly in the same way, except that the number of arguments depends on which event it is. For example, the `ready` event: 

```js
exports.run = (bot) => {
  console.log(`Ready to server in ${bot.channels.size} channels on ${bot.guilds.size} servers, for a total of ${bot.users.size} users.`);
}
```

Note that the `ready` event normally doesn't have any arguments, it's just (). But because we're in separate modules, it's necessary to "pass" the `bot` variable to it or it would not be accessible. 

Here's another example with the `guildMemberAdd` event: 

```js
exports.run = (bot, member) => {
  let guild = member.guild;
  guild.defaultChannel.sendMessage(`Welcome ${member.user} to this server.`).catch(console.error);
}
```

Now we have `bot` and also `member` which is the argument provided *by* the `guildMemberAdd` event.

## BONUS: The "reload" command

Because of the way `require()` works in node, if you modify any of the command files in `./commands` , the changes are not reflected immediately when you call that command again - because `require()` *caches* the file in memory instead of reading it every time. While this is great for efficiency, it means we need to clear that cached version if we change commands. 

The *Reload* command does just that, simply deletes the cache so the next time that specific command is run, it'll refresh its code from the file. 

```js
exports.run = (bot, message, args) => {
  if(!args || args.size < 1) return message.channel.reply(`Must provide a command name to reload.`);
  // the path is relative to the *current folder*, so just ./filename.js
  delete require.cache[require.resolve(`./${args[0]}.js`)];
  message.reply(`The command ${args[0]} has been reloaded`);
};
```