# 📜 Chamber Bot — Message Content Intent Proof

Hello Discord Trust & Safety Team,

This repository demonstrates why **Chamber** requires the **Message Content Intent**.

Chamber is a **voice channel automation platform (Join-to-Create)** that manages temporary voice channels and their associated text chats.

Because the main Chamber bot currently has **Message Content Intent disabled**, we implemented these features on a **testing bot (Chamber Beta)** to demonstrate the functionality.

The beta bot exists **only for demonstration purposes** so Discord reviewers can verify how the system works before enabling the intent on the production bot.

---

# 🔍 Scope Limitation

Message content is **only processed inside temporary voice channel text chats created by Chamber**.

The bot **does NOT read messages in normal server channels**.

Before processing any message, the bot validates that the message comes from a Chamber temporary voice channel text chat using the database:

```
VoiceChannel.findOne({ textChannelId: message.channel.id })
```

If the channel is **not linked to a Chamber temporary voice channel**, the message is ignored.

---

# 🎙 Features That Require Message Content

## 1️⃣ Voice Session Logging

Chamber generates a **voice session report** when a temporary voice channel session ends.

The bot counts activity in the voice channel text chat and records key events that occurred during the session.

Example session report:

```
Voice Session Report

Channel: demondev_'s Channel
Owner: demondev_
Duration: 41s
Participants: 2
Messages Sent: 2

Events:
VC Created
Automod Action
User rejected by automod
VC Locked
Limit Changed
User rejected by automod
Incident report created
```

Proof:

- [📷 Screenshot - VC Session Report](https://i.postimg.cc/T3PMnhnT/sessionlogs.png)
- [🎥 Video - VC Session Report](https://youtube.com/shorts/F-OxxHIEXT8?feature=share)

Message content is **not stored** — only activity statistics are recorded.

---

## 2️⃣ Voice Chat Moderation (Automod)

Chamber moderates temporary voice channel text chats to protect voice sessions.

Detected violations include:

• Discord invite links
• External links
• Mass mentions (`@everyone` / `@here`)
• Spam messages

Example violation:

```
discord.gg/example
```

Bot action:

• User is rejected from the voice channel
• User receives a DM explaining the moderation action
• Moderation log is sent to the configured log channel

Proof:

- [📷 Screenshot - VC Automod Dm Notificaiton](https://i.postimg.cc/DfYYR5G7/chamberautomoddm.png)
- [📷 Screenshot - VC Automod Logs](https://i.postimg.cc/x8kg0Dzw/automodlogs.png)
- [🎥 Video - VC Automod](https://youtube.com/shorts/3MiN9UnwZRk?feature=share)
---

## 3️⃣ Voice Channel Incident Reports

Users can report incidents occurring during a voice session.

Command example:

```
/incident report harassment
```

When used, Chamber gathers context from the **last 10 messages in the temporary VC text chat** and sends a report to moderators.

Example incident report:

```
Incident Report

Reporter: @ZAYANshk (zayan_shk)
Channel: zayan_shk's Channel
Keyword: hehe

Recent Messages
Chamber Beta: [no text content]
demondev_: Hey we are going to show incident report system
demondev_: hehe
demondev_: hehe
demondev_: hehehe
demondev_: report it
```

Proof:

- [📷 Screenshot - VC Incident Report](https://i.postimg.cc/x8kg0Dzw/automodlogs.png)
- [🎥 Video - VC Incident Report](https://youtube.com/shorts/3MiN9UnwZRk?feature=share)

---

# 🔒 Data Storage & Privacy

Chamber stores only minimal operational data required for bot functionality.

Stored data includes:

• Guild IDs
• User IDs
• Channel IDs
• Configuration settings
• Session statistics

The bot **does not permanently store message content or chat history**.

Temporary message context used for incident reports is **not stored after the report is generated**.

MongoDB is used for configuration storage and is secured with authentication.

---

# 🧪 Demonstration Bot

Because Message Content Intent is currently disabled on the production Chamber bot, these features are demonstrated using a **beta bot instance (Chamber Beta)**.

This beta bot exists only to demonstrate:

• Voice session logging
• Voice chat moderation
• Voice channel incident reporting

Once Message Content Intent is approved, these features will be integrated into the main Chamber bot.

---

# 📞 Contact

Support Server
https://discord.gg/VQFnjvvFhc

Developer
https://discord.com/users/555652788592443392

Email
[support@demondev.org](mailto:support@demondev.org)
