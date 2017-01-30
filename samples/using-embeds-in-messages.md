# Embeds and Messages

You've seen them all around - these sexy dark grey boxes with a nice color on the left, images, and **tables** oh my god. So nice-looking, right? Well, let me show you how to make them!

> **Fair Warning**: Embeds might look nice but they can be disabled through permissions and user preferences, and will not look the same on mobile - especially complex ones. It's strongly recommended *not* to use them unless you have a text-only fallback. Yes they're nice, but, don't use them if you don't *need* to!

## Object-based embeds

There are 2 ways to do embeds. The first, is by writing the embed yourself, as an object. Here's a very, *very* basic embed that writes on a single line: 

```js
msg.channel.sendMessage("", {embed: {
  color: 3447003,
  description: "A very simple Embed!"
}});
```
  
The `color` determines the bar on the left (here, a very nice blue), and the `description` is the main contents of the message. In embeds, every field is technically optional, but you need at least *one* field. 

Alternatively to sendMessage("", embed) you can also use the sendEmbed shortcut: 

```js
msg.channel.embed({ color: 3447003,
  description: "A very simple Embed!" });
```
