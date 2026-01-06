# TeleBlox

A Lua module for Roblox that lets you connect your game to a Telegram bot using the official Telegram Bot API. You can send messages, photos, videos, reactions, buttons, do polling, moderate chats — basically all the boring bot stuff, but from Roblox.

I made this so I wouldn’t have to rewrite the same Telegram wrapper again and again. It works, it’s not pretty in some places, but it does the job.

---

## What this thing does

* Connects Roblox to a Telegram bot
* Wraps **Telegram Bot API** methods
* Supports long polling (`getUpdates`)
* Sending messages, media, dice, reactions
* Inline keyboards and buttons
* Basic Telegram moderation (ban / unban / pin / leave)
* Editing messages
* Custom & emoji reactions

Working with HTTP-requests and saves your time

---

## Requirements

* Roblox game with **HttpService enabled**
* A Telegram bot token (from `@BotFather`)

---

## Installation

1. Copy the module code
2. Paste it into a **ModuleScript** in Roblox Studio
3. Require it where you need it

```lua
local TeleBlox = require(path.to.TeleBlox)
```

That’s it. No setup wizards, no npm, no bullshit.

---

## Declaring a bot

Before doing anything, you **must** declare your bot:

```lua
TeleBlox.DeclareBot("YOUR_BOT_TOKEN")
```

This also checks if the token is valid. If it explodes — token is wrong or HTTP is disabled.

---

## Basic usage

### Send a message

```lua
TeleBlox.SendMessage(true, chat_id, "Hello from Roblox")
```

`true` means: use the declared bot token.
You can also pass the token manually instead of `true`.

---

### Get bot info

```lua
local me = TeleBlox.GetMe(true)
print(me)
```

---

### Sending media

```lua
TeleBlox.SendPhoto(true, chat_id, "https://example.com/image.png")
TeleBlox.SendVideo(true, chat_id, video_url)
TeleBlox.SendAudio(true, chat_id, audio_url)
TeleBlox.SendDocument(true, chat_id, file_url)
```

Files must be **URLs or Telegram file_ids**.

---

## Inline buttons & keyboards

### Inline button

```lua
local button = TeleBlox.Types.InlineButton(
    "Click me",
    nil,
    "callback_data_here"
)
```

### Inline keyboard

```lua
local keyboard = TeleBlox.Types.InlineKeyboard(button)

TeleBlox.SendMessage(true, chat_id, "Message with button", nil, keyboard)
```

---

## Polling (getting updates)

Long polling loop. 

```lua
TeleBlox.PollingBot(true, function(update)
    print(update)
end, 0.5)
```

* `cooldown` is delay between requests
* Set polling to stop with:

```lua
TeleBlox.StopPolling()
```

This runs forever unless you stop it. 

---

## Reactions

### Emoji reaction

```lua
local reaction = TeleBlox.Types.ReactionTypeEmoji("🔥")
TeleBlox.SetMessageReaction(true, chat_id, message_id, reaction)
```

### Custom emoji reaction

```lua
local reaction = TeleBlox.Types.ReactionTypeCustomEmoji(custom_emoji_id)
TeleBlox.SetMessageReaction(true, chat_id, message_id, reaction)
```

---

## Chat moderation

```lua
TeleBlox.BanChatMember(true, chat_id, user_id)
TeleBlox.UnbanChatMember(true, chat_id, user_id)
TeleBlox.PinChatMessage(true, chat_id, message_id)
TeleBlox.UnpinChatMessage(true, chat_id, message_id)
TeleBlox.LeaveChat(true, chat_id)
```

Bot must have admin rights.

---

## Notes

* This module uses **GET requests** everywhere.
* Polling is not optimized. Don’t run it 24/7 for 500 bots.

If something breaks — check Telegram API docs first:
[https://core.telegram.org/bots/api/](https://core.telegram.org/bots/api/)

---

## Version

TeleBlox v1.2

---

## Links

Telegram channel - https://t.me/teleblox
Roblox asset - https://create.roblox.com/store/asset/11495711008/TeleBlox
