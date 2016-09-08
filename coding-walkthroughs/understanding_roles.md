# Understanding Roles

Roles are a powerful feature in Discord, and admittedly have been one of the hardest parts to master in discord.js. This walkthrough aims at explaining how roles and permissions work. We'll also explore how to use roles to protect your commands.

## Role hierarchy

Let's start with a basic overview of the hierarchy of roles in Discord. 

... or actually not, they already explain it better than I care to: [Role Management 101](https://support.discordapp.com/hc/en-us/articles/214836687-Role-Management-101). Read up on that, then come back here. I'll wait. (Yeah I know that's cheesy, so sue me).

## Some code!

Let's get down to the brass tax. You want to know how to use roles and permissions in your bot. Well, it's 1AM and I really want to get this info written down, but I promise to make some examples soon™

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
// Checks for Manage Messages permissions.
msg.channel.permissionsFor(msg.author).hasPermission('MANAGE_MESSAGES')
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

### Get all permissions of a member on a channel

You can get a list of permissions for a user on any specific channel by using the following code:

```js
msg.channel.permissionsFor(msg.member)
```

This will give you the permissions `calculated for that channel`, which you can parse yourself, view with `msg.channel.permissionsFor(msg.member).serialize()`.

### Get all permissions of a member on a guild

> Currently, this is **not possible** because Discord does not provide this information. Neither does it provide *valid* position data for each role, making it impossible to calculate. Go complain on Discord Developpers! :D