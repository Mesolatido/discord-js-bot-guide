# Understanding Roles

Roles are a powerful feature in Discord, and admittedly have been one of the hardest parts to master in discord.js. This walkthrough aims at explaining how roles and permissions work. We'll also explore how to use roles to protect your commands.

## Role hierarchy

Let's start with a basic overview of the hierarchy of roles in Discord. 

... or actually not, they already explain it better than I care to: [Role Management 101](https://support.discordapp.com/hc/en-us/articles/214836687-Role-Management-101). Read up on that, then come back here. I'll wait. (Yeah I know that's cheesy, so sue me).

## Some code!

Let's get down to the brass tax. You want to know how to use roles and permissions in your bot.

### Get Role by Name or ID

This is the "easy" part once you actually get used to it. It's just like getting any other Collection element, but here's a reminder anyway!

```js
// get role by ID
let myRole = msg.guild.roles.get("222089067028807682");

// get role by name
let myRole = msg.guild.roles.find("name", "Mods");
```

### Check if a member has a role
In a `message` handler, you have access to checking the GuildMember class of the message author:

```js
// assuming role.id is an actual ID of a valid role:
if(msg.member.roles.has(role.id)) {
  console.log(`Yay, the author of the message has the role!`);
} else {
  console.log(`Nope, noppers, nadda.`);
}
```

> BTW mentions are `User` objects, not `GuildMember` but you can resolve that by doing `msg.guild.member(msg.mentions.users.first())` to get the first mentions' `GuildMember` object.

### Get all members that have a role
You can get a list of members in your guild that have a specific role by filtering the guild members.

> There's plans for a `<Guild>.usersWithRole()` function at a later date to simplify this!

```js
let roleID = "216219836957720576";
let membersWithRole = msg.guild.members.filter(m=> m.roles.has(roleID))
console.log(`Got ${membersWithRole.size} members with that role.`);
```

### Check specific permission of a member on a channel
To check for a single permission override on a channel:

```js
// Getting all permissions for a member on a channel.
let perms = msg.channel.permissionsFor(msg.member);

// Checks for Manage Messages permissions.
let can_manage_chans = msg.channel.permissionsFor(msg.member).hasPermission("MANAGE_CHANNELS");

// View permissions as an object (useful for debugging or eval)
msg.channel.permissionsFor(msg.member).serialize()
```

### Check specific permission of a member on a server
At the moment there is no easy helper to check for a specific permission. While we work on that, here's a longer/manual way:

```js
if( msg.member.roles.filter(r=>r.hasPermission('KICK_MEMBERS')).size > 0) {
  console.log(`Member has the KICK_MEMBERS permissions! Kick away!`);
} else {
  console.log(`No Perm For U`);
}
```

### Get all permissions of a member on a guild

Just as easy, wooh! 

```js
let perms = msg.member.permissions;
let has_kick = msg.member.hasPermission("KICK_MEMBERS");
```

ezpz, right?

Now get to coding!