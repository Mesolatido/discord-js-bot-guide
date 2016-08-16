# Getting Audio to Work

In the **Getting Started** guides, I mention briefly the pre-requisites necessary for audio to work. However, before we get started looking at the code necessary to actually execute a music bot, we have to make audio work on your system. 

> NONE of the steps outlined below should be skipped for your operating system. Every single one of them is important.

## Linux Users
In 90% of the case, following the [Getting Started - TL;DR](getting-started-linux-tldr.md) instructions should be fine to get audio working. There are 2 exceptions to this, however, depending on your system.

### Raspberri Pi 1st Gen
Though the rPI 1st gen is getting old (5 years), it's still possible to run a bot on it. However because of its age and its armv6 architecture, it's unfortunately not possible to install node 4 or 6 with the "default" instructions.

> These instructions are for Rasbpian Jessi 8, which I strongly suggest using.

Start by removing the previous version of node (0.10) using the following command:
`sudo apt-get remove --purge node* npm*`

You then need to download & extract the correct file for this system:

```
wget https://nodejs.org/dist/latest-v6.x/node-v6.3.1-linux-armv6l.tar.xz
sudo apt-get install xz-utils
tar -C /usr/local --strip-components 1 -xJf node-v6.3.1-linux-armv6l.tar.xz
```

(go to section below next)

### All rPI users, and Ubuntu 14.04

For both Ubuntu 14.04 and Rasbpian Jessie 8, the `ffmpeg` package is not directly available in `apt-get`. Instead of having you *build* it though, we can use an alternative package that works just as well: 

`sudo apt-get install libav-tools`

> Discord.js is set to use *either* `avconv` which is part of libav-tools, or `ffmpeg`. There is no need for anything else.


## Windows Users

> **DISCLAIMER**: Music bots under windows are THE hardest thing to get working. Not to code, but to **get working**. So, fair warning, I write this guide while completely despising Windows as a development environment. If the below instructions do not work, **USE LINUX**. This guide was fully tested, and works for most people. If this guide can't help you, no one can.

### The easy way

Because of changes in Microsoft's development ecosystem, it *should* be a relative breeze to install the appropriate requirements. If you're lucky, all you need to do is: 

- FFMPEG, which needs to be downloaded, extracted, and added to path:
  - Download either the 32 or 64 bit **static** version from [here](https://ffmpeg.zeranoe.com/builds/)
  - Unzip the contents of the zip file to an easy location like C:\ffmpeg\ or something.
  - Add the location where you unzipped the file, to your path (see [these instructions, step 3](http://adaptivesamples.com/how-to-install-ffmpeg-on-windows/))
- The Build Tools:
  - Make sure you have Node 4 or 6 installed (obviously).
  - Open an *Admin* command prompt or *PowerShell*.
  - Type in the following command: `npm i -g --production windows-build-tools`
  - Wait for it to finish (could be a few minutes)
  - Close this window.

Once that's done, execute the following codes in your bot's folder:

```
npm i -g npm
npm clean cache
npm i discord.js
```
