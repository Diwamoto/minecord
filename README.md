# Minecord

Connects [Minecraft](https://minecraft.net) Server and [Discord](https://discordapp.com/) without any mods or plugins.

![Minecord](https://raw.githubusercontent.com/wiki/node-link/minecord/images/minecord.gif)

## Installation

```
npm install -g minecord
```

## Usage

In your Minecraft `server.properties`, make sure you have and restart the server.

```
enable-rcon=true
rcon.password=[minecraftRconPassword]
rcon.port=[minecraftRconPort]
```

Create a bot of Discord from [here](https://discordapp.com/developers/applications/me) and get bot token.

Then let the created Bot participate in a specific channel from the OAuth 2 URL Generator.

Set up the `config.json` file.

```json
{
  "pluginsDir": null,
  "enable": [
    "chat",
    "login",
    "whitelist",
    "server"
  ],
  "disable": [],
  "minecraftLog": "/var/minecraft/logs/latest.log",
  "minecraftRconHost": "localhost",
  "minecraftRconPort": 25575,
  "minecraftRconPassword": "password",
  "discordBotToken": "bot_token",
  "discordChannel": "channel_id"
}
```

In order to acquire the channel ID, enable developer mode in your Discord client, then right click channel and select "Copy ID".

And run Minecord.

```
minecord --config /path/to/config.json
```

## Plugins

By default, the following plugins are included.

* chat : Sharing messages entered by users with Minecraft and Discord on their respective screens. (Default: enable)
* login : Transfer login notification to Minecraft to Discord. (Default: disable)
* whitelist : Transfer whitelist operation notification in Minecraft to Discord. (Default: disable)
* server : Transfer notification of start and stop of Minecraft server to Discord. (Default: disable)

## How to make a plugin

To load the local plugin, please put the plugin in the directory specified by the `--plugins-dir` option and specify it in the configuration file.

When publishing the global plugin, please publish it as `minecord-plugin-[PLUGIN]`.

```js
export default Plugin => new Plugin({
  discord ({message, sendToDiscord, sendToMinecraft}) {
    // Processing when receiving a message from Discord.
  },
  minecraft ({log, time, causedAt, level, message, sendToDiscord, sendToMinecraft}) {
    // Processing when receiving a message from Minecraft.
  }
})
```

### `discord()` method

It is executed when a message is received from the Discord channel.

Argument `message` is [`Message`](https://discord.js.org/#/docs/main/stable/class/Message) object of [discord.js](https://discord.js.org).

Argument `sendToDiscord` is [`send`](https://discord.js.org/#/docs/main/stable/class/TextChannel?scrollTo=send) function of [discord.js](https://discord.js.org).

Argument `sendToMinecraft` is `send` function of [node-modern-rcon](https://github.com/levrik/node-modern-rcon).

### `minecraft()` method

It is executed when new Minecraft log is detected.

Argument `log` is one Minecraft log line.

Argument `time`, `causedAt`, `level` and `message` is the value obtained by parsing the Minecraft log.

For example, it becomes as follows.

```
[01:23:45] [Server thread/INFO]: player joined the game
```

```js
export default Plugin => new Plugin({
  minecraft ({log, time, causedAt, level, message, sendToDiscord, sendToMinecraft}) {
    console.log(log === '[01:23:45] [Server thread/INFO]: player joined the game')
    console.log(time === '01:23:45')
    console.log(causedAt === 'Server thread')
    console.log(level === 'INFO')
    console.log(message === 'player joined the game')
    // All true
  }
})
```

Argument `sendToDiscord` is [`send`](https://discord.js.org/#/docs/main/stable/class/TextChannel?scrollTo=send) function of [discord.js](https://discord.js.org).

Argument `sendToMinecraft` is `send` function of [node-modern-rcon](https://github.com/levrik/node-modern-rcon).

## Contribution

I would be delighted if you rewrite `README.md` in native English!!

Of course, I would be delighted if you point out any problems or improvements related to the program.

https://github.com/node-link/minecord/issues
