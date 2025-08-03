# <div align='center'>Baileys @dimasmpp130/baileys-news</div>

<div align="center">
  <img src="https://e.top4top.io/p_3502kfjks1.jpg" />
  <a href="https://github.com/dimasmpp130/baileys-news">
    <img src="https://img.shields.io/github/languages/code-size/dimasmpp130/baileys-news" />
  </a>
  <a href="LICENSE">
    <img src="https://img.shields.io/badge/license-GPL%203-blue.svg" />
  </a>
  <a href="https://whatsapp.com/channel/0029Vad5qbBChq6OisE9Ot33">
    <img src="https://img.shields.io/badge/WhatsApp-Channel-25D366?logo=whatsapp&logoColor=white" alt="WhatsApp Channel" />
  </a>
</div>

<div align='center'>@dimasmpp130/baileys-news is a WebSockets-based TypeScript library for interacting with the WhatsApp Web API. This library has been enhanced to address issues with WhatsApp group `@lid` or `lid` handling, ensuring robust performance.</div>

# Key Features

- **Fixed @lid & @jid** - Resolved WhatsApp group `@lid` or `lid` issues
- **Multi-Device Support** - Supports WhatsApp multi-device connections
- **End-to-End Encryption** - Fully encrypted communications
- **All Message Types** - Supports all message types (text, media, polls, etc.)
- **Easy to Use** - Simple and intuitive API

# Disclaimer

The original repository was initially removed by its creator and subsequently taken over by [WhiskeySockets](https://github.com/WhiskeySockets). Building upon this foundation, I have implemented several enhancements and introduced new features that were not present in the original repository. These improvements aim to elevate functionality and provide a more robust and versatile experience.

The maintainers of `@dimasmpp130/baileys-news` do not condone the use of this application in practices that violate WhatsApp's Terms of Service. We urge users to exercise personal responsibility and use this application fairly and responsibly.

**Use wisely. Avoid spamming. Refrain from excessive automated usage.**

## Install

### Install In package.json
```json
"dependencies": {
    "@whiskeysockets/baileys": "github:dimasmpp130/baileys-news"
}
```

### Stable Version (Recommended)
```bash
yarn install github:dimasmpp130/baileys-news
# or
yarn add github:dimasmpp130/baileys-news
```

### Edge Version (Latest Features)
```bash
yarn install github:dimasmpp130/baileys-news@latest
# or
yarn add github:dimasmpp130/baileys-news@latest
```

## Connecting Account

WhatsApp provides a multi-device API that allows Baileys to be authenticated as a second WhatsApp client by scanning a **QR code** or **Pairing Code** with WhatsApp on your phone.

> [!NOTE]
> **[Here](#example-to-start) is a simple example of event handling**

### Starting socket with **QR-CODE**

```javascript
// you can use this package to export a base64 image or a canvas element.
const qrcode = require("qrcode-terminal")

sock.ev.on('connection.update', async (update) => {
  const {connection, lastDisconnect, qr } = update
  // on a qr event, the connection and lastDisconnect fields will be empty

  // In prod, send this string to your frontend then generate the QR there
  if (qr) {
    // as an example, this prints the qr code to the terminal
    console.log(await qrcode.generate(qr, { small: true }))
  }
})
```

If the connection is successful, you will see a QR code printed on your terminal screen, scan it with WhatsApp on your phone and you'll be logged in!

### Starting socket with **Pairing Code**

> [!IMPORTANT]
> Pairing Code isn't Mobile API, it's a method to connect Whatsapp Web without QR-CODE, you can connect only with one device, see [here](https://faq.whatsapp.com/1324084875126592/?cms_platform=web)

The phone number can't have `+` or `()` or `-`, only numbers, you must provide country code

```javascript
sock.ev.on('connection.update', async (update) => {
  const {connection, lastDisconnect, qr } = update
  if (connection == "connecting" || !!qr) { // your choice
    const code = await sock.requestPairingCode(phoneNumber)
    // send the pairing code somewhere
  }
})
```

# Table of Contents

- [Connecting Account](#connecting-account)
    - [Connect with QR-CODE](#starting-socket-with-qr-code)
    - [Connect with Pairing Code](#starting-socket-with-pairing-code)
    - [Receive Full History](#receive-full-history)
- [Important Notes About Socket Config](#important-notes-about-socket-config)
    - [Caching Group Metadata (Recommended)](#caching-group-metadata-recommended)
    - [Improve Retry System & Decrypt Poll Votes](#improve-retry-system--decrypt-poll-votes)
    - [Receive Notifications in Whatsapp App](#receive-notifications-in-whatsapp-app)

- [Save Auth Info](#saving--restoring-sessions)
- [Handling Events](#handling-events)
    - [Example to Start](#example-to-start)
    - [Decrypt Poll Votes](#decrypt-poll-votes)
    - [Summary of Events on First Connection](#summary-of-events-on-first-connection)
- [Implementing a Data Store](#implementing-a-data-store)
- [Whatsapp IDs Explain](#whatsapp-ids-explain)
- [Utility Functions](#utility-functions)
- [Sending Messages](#sending-messages)
    - [Non-Media Messages](#non-media-messages)
        - [Text Message](#text-message)
        - [Quote Message](#quote-message-works-with-all-types)
        - [Mention User](#mention-user-works-with-most-types)
        - [Forward Messages](#forward-messages)
        - [Location Message](#location-message)
        - [Contact Message](#contact-message)
        - [Reaction Message](#reaction-message)
        - [Pin Message](#pin-message)
        - [Poll Message](#poll-message)
    - [Sending with Link Preview](#sending-messages-with-link-previews)
    - [Media Messages](#media-messages)
        - [Gif Message](#gif-message)
        - [Video Message](#video-message)
        - [Audio Message](#audio-message)
        - [Image Message](#image-message)
        - [ViewOnce Message](#view-once-message)
- [Modify Messages](#modify-messages)
    - [Delete Messages (for everyone)](#deleting-messages-for-everyone)
    - [Edit Messages](#editing-messages)
- [Manipulating Media Messages](#manipulating-media-messages)
    - [Thumbnail in Media Messages](#thumbnail-in-media-messages)
    - [Downloading Media Messages](#downloading-media-messages)
    - [Re-upload Media Message to Whatsapp](#re-upload-media-message-to-whatsapp)
- [Reject Call](#reject-call)
- [Send States in Chat](#send-states-in-chat)
    - [Reading Messages](#reading-messages)
    - [Update Presence](#update-presence)
- [Modifying Chats](#modifying-chats)
    - [Archive a Chat](#archive-a-chat)
    - [Mute/Unmute a Chat](#muteunmute-a-chat)
    - [Mark a Chat Read/Unread](#mark-a-chat-readunread)
    - [Delete a Message for Me](#delete-a-message-for-me)
    - [Delete a Chat](#delete-a-chat)
    - [Star/Unstar a Message](#starunstar-a-message)
    - [Disappearing Messages](#disappearing-messages)
- [User Querys](#user-querys)
    - [Check If ID Exists in Whatsapp](#check-if-id-exists-in-whatsapp)
    - [Query Chat History (groups too)](#query-chat-history-groups-too)
    - [Fetch Status](#fetch-status)
    - [Fetch Profile Picture (groups too)](#fetch-profile-picture-groups-too)
    - [Fetch Bussines Profile (such as description or category)](#fetch-bussines-profile-such-as-description-or-category)
    - [Fetch Someone's Presence (if they're typing or online)](#fetch-someones-presence-if-theyre-typing-or-online)
- [Change Profile](#change-profile)
    - [Change Profile Status](#change-profile-status)
    - [Change Profile Name](#change-profile-name)
    - [Change Display Picture (groups too)](#change-display-picture-groups-too)
    - [Remove display picture (groups too)](#remove-display-picture-groups-too)
- [Groups](#groups)
    - [Create a Group](#create-a-group)
    - [Add/Remove or Demote/Promote](#addremove-or-demotepromote)
    - [Change Subject (name)](#change-subject-name)
    - [Change Description](#change-description)
    - [Change Settings](#change-settings)
    - [Leave a Group](#leave-a-group)
    - [Get Invite Code](#get-invite-code)
    - [Revoke Invite Code](#revoke-invite-code)
    - [Join Using Invitation Code](#join-using-invitation-code)
    - [Get Group Info by Invite Code](#get-group-info-by-invite-code)
    - [Query Metadata (participants, name, description...)](#query-metadata-participants-name-description)
    - [Join using groupInviteMessage](#join-using-groupinvitemessage)
    - [Get Request Join List](#get-request-join-list)
    - [Approve/Reject Request Join](#approvereject-request-join)
    - [Get All Participating Groups Metadata](#get-all-participating-groups-metadata)
    - [Toggle Ephemeral](#toggle-ephemeral)
    - [Change Add Mode](#change-add-mode)
- [Newsletter](#newsletter)
    - [Subscribe to Newsletter Updates](#subscribe-to-newsletter-updates)
    - [Update Reaction Mode](#update-reaction-mode)
    - [Update Newsletter Description](#update-newsletter-description)
    - [Update Newsletter Name](#update-newsletter-name)
    - [Update Newsletter Picture](#update-newsletter-picture)
    - [Remove Newsletter Picture](#remove-newsletter-picture)
    - [Follow a Newsletter](#follow-a-newsletter)
    - [Unfollow a Newsletter](#unfollow-a-newsletter)
    - [Mute a Newsletter](#mute-a-newsletter)
    - [Unmute a Newsletter](#unmute-a-newsletter)
    - [Perform a Newsletter Action (Generic)](#perform-a-newsletter-action-generic)
    - [Create a Newsletter](#create-a-newsletter)
    - [Fetch Newsletter Metadata](#fetch-newsletter-metadata)
    - [Get Admin Count](#get-admin-count)
    - [Change Newsletter Owner](#change-newsletter-owner)
    - [Demote Newsletter Admin](#demote-newsletter-admin)
    - [Delete Newsletter](#delete-newsletter)
    - [React to a Message in Newsletter](#react-to-a-message-in-newsletter)
    - [Fetch Messages in Newsletter](#fetch-messages-in-newsletter)
    - [Fetch Updates in Newsletter](#fetch-updates-in-newsletter)
- [Privacy](#privacy)
    - [Block/Unblock User](#blockunblock-user)
    - [Get Privacy Settings](#get-privacy-settings)
    - [Get BlockList](#get-blocklist)
    - [Update LastSeen Privacy](#update-lastseen-privacy)
    - [Update Online Privacy](#update-online-privacy)
    - [Update Profile Picture Privacy](#update-profile-picture-privacy)
    - [Update Status Privacy](#update-status-privacy)
    - [Update Read Receipts Privacy](#update-read-receipts-privacy)
    - [Update Groups Add Privacy](#update-groups-add-privacy)
    - [Update Default Disappearing Mode](#update-default-disappearing-mode)
- [Broadcast Lists & Stories](#broadcast-lists--stories)
    - [Send Broadcast & Stories](#send-broadcast--stories)
    - [Query a Broadcast List's Recipients & Name](#query-a-broadcast-lists-recipients--name)
- [Writing Custom Functionality](#writing-custom-functionality)
    - [Enabling Debug Level in Baileys Logs](#enabling-debug-level-in-baileys-logs)
    - [How Whatsapp Communicate With Us](#how-whatsapp-communicate-with-us)
    - [Register a Callback for Websocket Events](#register-a-callback-for-websocket-events)

### Receive Full History

1. Set `syncFullHistory` as `true`
2. Baileys, by default, use chrome browser config
    - If you'd like to emulate a desktop connection (and receive more message history), this browser setting to your Socket config:

```javascript
const { Browsers } = require("@whiskeysockets/baileys")
const makeWASocket = require("@whiskeysockets/baileys").default

const sock = makeWASocket({
    ...otherOpts,
    // can use Windows, Ubuntu here too
    browser: Browsers.chrome('Baileys'),
    syncFullHistory: true
})
```

## Important Notes About Socket Config

### Caching Group Metadata (Recommended)
- If you use baileys for groups, we recommend you to set `cachedGroupMetadata` in socket config, you need to implement a cache like this:

    ```javascript
    const makeWASocket = require("@whiskeysockets/baileys").default
    const NodeCache = require("node-cache")
    const groupCache = new NodeCache({stdTTL: 5 * 60, useClones: false})

    const sock = makeWASocket({
        cachedGroupMetadata: async (jid) => groupCache.get(jid)
    })

    sock.ev.on('groups.update', async ([event]) => {
        const metadata = await sock.groupMetadata(event.id)
        groupCache.set(event.id, metadata)
    })

    sock.ev.on('group-participants.update', async (event) => {
        const metadata = await sock.groupMetadata(event.id)
        groupCache.set(event.id, metadata)
    })
    ```

### Improve Retry System & Decrypt Poll Votes
- If you want to improve sending message, retrying when error occurs and decrypt poll votes, you need to have a store and set `getMessage` config in socket like this:
    ```javascript
    const makeWASocket = require("@whiskeysockets/baileys").default
    
    const sock = makeWASocket({
        getMessage: async (key) => {
            if (store) {
                const msg = await store.loadMessage(key.remoteJid, key.id)
                return msg?.message || undefined
            }
            return proto.Message.fromObject({})
        }
    })
    ```
    For the **store.loadMessages** function, you can create it yourself using a database or a database from Monggo, I have already created it there so I will just include it as an example.

### Receive Notifications in Whatsapp App
- If you want to receive notifications in whatsapp app, set `markOnlineOnConnect` to `false`
    ```javascript
    const makeWASocket = require("@whiskeysockets/baileys").default
    
    const sock = makeWASocket({
        markOnlineOnConnect: false
    })
    ```
## Saving & Restoring Sessions

You obviously don't want to keep scanning the QR code every time you want to connect. 

So, you can load the credentials to log back in:
```javascript
const { useMultiFileAuthState, makeCacheableSignalKeyStore } = require("@whiskeysockets/baileys")
const makeWASocket = require("@whiskeysockets/baileys").default

const { state, saveCreds } = await useMultiFileAuthState('./auth_info_baileys')

// will use the given state to connect
// so if valid credentials are available -- it'll connect without QR
const sock = makeWASocket({
  auth: {
    creds: state.creds,
    // use makeCacheableSignalKeyStore
    keys: makeCacheableSignalKeyStore(
      state.keys, 
      pino().child({ level: "silent", stream: "store" }),
    )
  }
})

// this will be called as soon as the credentials are updated
sock.ev.on('creds.update', saveCreds)
```

> [!IMPORTANT]
> `useMultiFileAuthState` is a utility function to help save the auth state in a single folder, this function serves as a good guide to help write auth & key states for SQL/no-SQL databases, which I would recommend in any production grade system.

> [!NOTE]
> When a message is received/sent, due to signal sessions needing updating, the auth keys (`authState.keys`) will update. Whenever that happens, you must save the updated keys (`authState.keys.set()` is called). Not doing so will prevent your messages from reaching the recipient & cause other unexpected consequences. The `useMultiFileAuthState` function automatically takes care of that, but for any other serious implementation -- you will need to be very careful with the key state management.

## Handling Events

- Baileys uses the EventEmitter syntax for events. 
They're all nicely typed up, so you shouldn't have any issues with an Intellisense editor like VS Code.

You can listen to these events like this:
```javascript
const makeWASocket = require("@whiskeysockets/baileys").default

const sock = makeWASocket({
  auth: {
    creds: state.creds,
    // use makeCacheableSignalKeyStore
    keys: makeCacheableSignalKeyStore(
      state.keys, 
      pino().child({ level: "silent", stream: "store" }),
    )
  }
})
sock.ev.on('messages.upsert', ({ messages }) => {
    console.log('got messages', messages)
})
```

### Example to Start

> [!NOTE]
> This example includes basic auth storage too

```javascript
const { DisconnectReason, useMultiFileAuthState } = require("@whiskeysockets/baileys")
const makeWASocket = require("@whiskeysockets/baileys").default
const { Boom } = require("@hapi/boom")

async function connectToWhatsApp () {
    const { state, saveCreds } = await useMultiFileAuthState('./auth_info_baileys')
    // can provide additional config here
    const sock = makeWASocket({ auth: { creds: state.creds, keys: makeCacheableSignalKeyStore(state.keys, pino().child({ level: "silent", stream: "store" }))}})
    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update
        if(connection === 'close') {
            const shouldReconnect = (lastDisconnect.error as Boom)?.output?.statusCode !== DisconnectReason.loggedOut
            console.log('connection closed due to ', lastDisconnect.error, ', reconnecting ', shouldReconnect)
            // reconnect if not logged out
            if(shouldReconnect) {
                connectToWhatsApp()
            }
        } else if(connection === 'open') {
            console.log('opened connection')
        }
    })
    sock.ev.on('messages.upsert', event => {
        for (const m of event.messages) {
            console.log(JSON.stringify(m, undefined, 2))

            console.log('replying to', m.key.remoteJid)
            await sock.sendMessage(m.key.remoteJid!, { text: 'Hello Word' })
        }
    })

    // to storage creds (session info) when it updates
    sock.ev.on('creds.update', saveCreds)
}
// run in main file
connectToWhatsApp()
```

> [!IMPORTANT]
> In `messages.upsert` it's recommended to use a loop like `for (const message of event.messages)` to handle all messages in array

### Decrypt Poll Votes

- By default poll votes are encrypted and handled in `messages.update`
- That's a simple example
```javascript
sock.ev.on('messages.update', event => {
    for(const { key, update } of event) {
        if(update.pollUpdates) {
            const pollCreation = await getMessage(key)
            if(pollCreation) {
                console.log(
                    'got poll update, aggregation: ',
                    getAggregateVotesInPollMessage({
                        message: pollCreation,
                        pollUpdates: update.pollUpdates,
                    })
                )
            }
        }
    }
})
```

### Summary of Events on First Connection

1. When you connect first time, `connection.update` will be fired requesting you to restart sock
2. Then, history messages will be received in `messaging.history-set`

## Whatsapp IDs Explain

WhatsApp IDs (`jid`) formats:

- **Personal Chat**: `[country code][phone number]@s.whatsapp.net` (e.g., `1234567890@s.whatsapp.net`)
- **Group Chat** (Fixed `@jid`): `112233445566778899@g.us`
- **Broadcast**: `[timestamp]@broadcast` (e.g., `1234567890@broadcast`)
- **Status/Story**: `status@broadcast`
- **Newsletter**: `1234567890@newsletter`

> [!NOTE]
> `@lid` and `@jid` issues in groups are fixed in this library.

## Utility Functions

- `getContentType`, returns the content type for any message
- `getDevice`, returns the device from message
- `makeCacheableSignalKeyStore`, make auth store more fast
- `downloadContentFromMessage`, download content from any message

### Non-Media Messages

#### Text Message
```javascript
await sock.sendMessage(jid, { text: 'hello word' })
```

#### Quote Message (works with all types)
```javascript
await sock.sendMessage(jid, { text: 'hello word' }, { quoted: message })
```

#### Mention User (works with most types)
- @number is to mention in text, it's optional
```javascript
await sock.sendMessage(
    jid,
    {
        text: 'hello word',
        mentions: ['12345678901@s.whatsapp.net']
    }
)
```

#### Location Message
```javascript
await sock.sendMessage(
    jid, 
    {
        location: {
            degreesLatitude: 24.121231,
            degreesLongitude: 55.1121221
        }
    }
)
```
#### Contact Message
```javascript
const vcard = 'BEGIN:VCARD\n' // metadata of the contact card
            + 'VERSION:3.0\n' 
            + 'FN: Baileys Dimzzz\n' // full name
            + 'ORG:Hello Word;\n' // the organization of the contact
            + 'TEL;type=CELL;type=VOICE;waid=1234567890:+62 812345 67890\n' // WhatsApp ID + phone number
            + 'END:VCARD'

await sock.sendMessage(
    id,
    { 
        contacts: { 
            displayName: 'Test', 
            contacts: [{ vcard }] 
        }
    }
)
```

#### Reaction Message
```javascript
await sock.sendMessage(
    jid,
    {
        react: {
            text: '🗿', // use an empty string to remove the reaction
            key: message.key
        }
    }
)
```

#### Pin Message
- Time can be:

| Time  | Seconds        |
|-------|----------------|
| 24h    | 86.400        |
| 7d     | 604.800       |
| 30d    | 2.592.000     |

```javascript
await sock.sendMessage(
    jid,
    {
        pin: {
            type: 1, // 0 to remove
            time: 86400
            key: message.key
        }
    }
)
```

#### Poll Message
```javascript
await sock.sendMessage(
    jid,
    {
        poll: {
            name: 'My Poll',
            values: ['Option 1', 'Option 2', ...],
            selectableCount: 1,
            toAnnouncementGroup: false // or true
        }
    }
)
```

### Sending Messages with Link Previews

1. By default, wa does not have link generation when sent from the web
2. Baileys has a function to generate the content for these link previews
3. To enable this function's usage, add `link-preview-js` as a dependency to your project with `yarn add link-preview-js`
4. Send a link:
```javascript
await sock.sendMessage(
    jid,
    {
        text: 'Hi, this was sent using https://github.com/dimasmpp130/baileys-news'
    }
)
```

### Media Messages

Sending media (video, stickers, images) is easier & more efficient than ever.

- When specifying a media url, Baileys never loads the entire buffer into memory; it even encrypts the media as a readable stream.

> [!TIP]
> It's recommended to use Stream or Url to save memory

#### Gif Message
```javascript
await sock.sendMessage(
    jid, 
    { 
        video: {
            url: './Media/ma_gif.mp4'
        },
        caption: 'hello word',
        gifPlayback: true,
        mimetype: 'video/mp4'
    }
)
```

#### Default video Message
```javascript
await sock.sendMessage(
    jid, 
    { 
        video: {
            url: './Media/ma_gif.mp4'
        },
        caption: 'hello word',
        mimetype: 'video/mp4'
    }
)
```

#### Video Note Message
```javascript
await sock.sendMessage(
    id, 
    { 
        video: {
            url: './Media/ma_gif.mp4'
        },
        mimetype: 'video/mp4'
        caption: 'hello word',
	    ptv: false // if set to true, will send as a `video note`
    }
)
```

#### Audio Message
- To audio message work in all devices you need to convert with some tool like `ffmpeg` with this flags:
    ```bash
        codec: libopus //ogg file
        ac: 1 //one channel
        avoid_negative_ts
        make_zero
    ```
    - Example:
    ```bash
    ffmpeg -i input.mp4 -avoid_negative_ts make_zero -ac 1 output.ogg
    ```
```javascript
await sock.sendMessage(
    jid, 
    {
        audio: {
            url: './Media/audio.mp3'
        },
        mimetype: 'audio/mp4'
    }
)
```

#### Image Message
```javascript
await sock.sendMessage(
    id, 
    { 
        image: {
            url: './Media/ma_img.png'
        },
        caption: 'hello word'
    }
)
```

#### View Once Message

- You can send all messages above as `viewOnce`, you only need to pass `viewOnce: true` in content object

```javascript
await sock.sendMessage(
    id, 
    { 
        image: {
            url: './Media/ma_img.png'
        },
        viewOnce: true, // works with video, audio too
        caption: 'hello word'
    }
)
```

## Modify Messages

### Deleting Messages (for everyone)

```javascript
const msg = await sock.sendMessage(jid, { text: 'hello word' })
await sock.sendMessage(jid, { delete: msg.key })
```

**Note:** deleting for oneself is supported via `chatModify`, see in [this section](#modifying-chats)

### Editing Messages

- You can pass all editable contents here
```javascript
await sock.sendMessage(jid, {
      text: 'updated text goes here',
      edit: response.key,
    });
```

## Manipulating Media Messages

### Thumbnail in Media Messages
- For media messages, the thumbnail can be generated automatically for images & stickers provided you add `jimp` or `sharp` as a dependency in your project using `yarn add jimp` or `yarn add sharp`.
- Thumbnails for videos can also be generated automatically, though, you need to have `ffmpeg` installed on your system.

### Downloading Media Messages

If you want to save the media you received
```javascript
const { createWriteStream } = require("fs");
const { downloadMediaMessage, getContentType } = require("@whiskeysockets/baileys");

sock.ev.on('messages.upsert', async ({ [m] }) => {
    if (!m.message) return // if there is no text or media message
    const messageType = getContentType(m) // get what type of message it is (text, image, video...)

    // if the message is an image
    if (messageType === 'imageMessage') {
        // download the message
        const stream = await downloadMediaMessage(
            m,
            'stream', // can be 'buffer' too
            { },
            { 
                logger,
                // pass this so that baileys can request a reupload of media
                // that has been deleted
                reuploadRequest: sock.updateMediaMessage
            }
        )
        // save to file
        const writeStream = createWriteStream('./my-download.jpeg')
        stream.pipe(writeStream)
    }
}
```

### Re-upload Media Message to Whatsapp

- WhatsApp automatically removes old media from their servers. For the device to access said media -- a re-upload is required by another device that has it. This can be accomplished using: 
```javascript
await sock.updateMediaMessage(msg)
```

## Reject Call

- You can obtain `callId` and `callFrom` from `call` event

```javascript
await sock.rejectCall(callId, callFrom)
```

## Send States in Chat

### Reading Messages
```javascript
const key: WAMessageKey
// can pass multiple keys to read multiple messages as well
await sock.readMessages([key])
```

The message ID is the unique identifier of the message that you are marking as read. 
On a `WAMessage`, the `messageID` can be accessed using ```messageID = message.key.id```.

### Update Presence

- The presence expires after about 10 seconds.
- This lets the person/group with `jid` know whether you're online, offline, typing etc. 

```javascript
await sock.sendPresenceUpdate('available', jid) 
```

> [!NOTE]
> If a desktop client is active, WA doesn't send push notifications to the device. If you would like to receive said notifications -- mark your Baileys client offline using `sock.sendPresenceUpdate('unavailable')`

## Modifying Chats

WA uses an encrypted form of communication to send chat/app updates. This has been implemented mostly and you can send the following updates:

> [!IMPORTANT]
> If you mess up one of your updates, WA can log you out of all your devices and you'll have to log in again.

### Archive a Chat
```javascript
const lastMsgInChat = await getLastMessageInChat(jid) // implement this on your end
await sock.chatModify({ archive: true, lastMessages: [lastMsgInChat] }, jid)
```
### Mute/Unmute a Chat

- Supported times:

| Time  | Miliseconds     |
|-------|-----------------|
| Remove | null           |
| 8h     | 86.400.000     |
| 7d     | 604.800.000    |

```javascript
// mute for 8 hours
await sock.chatModify({ mute: 8 * 60 * 60 * 1000 }, jid)
// unmute
await sock.chatModify({ mute: null }, jid)
```
### Mark a Chat Read/Unread
```javascript
const lastMsgInChat = await getLastMessageInChat(jid) // implement this on your end
// mark it unread
await sock.chatModify({ markRead: false, lastMessages: [lastMsgInChat] }, jid)
```

### Delete a Message for Me
```javascript
await sock.chatModify(
    {
        clear: {
            messages: [
                {
                    id: 'ATWYHDNNWU81732J',
                    fromMe: true, 
                    timestamp: '1654823909'
                }
            ]
        }
    }, 
    jid
)

```
### Delete a Chat
```javascript
const lastMsgInChat = await getLastMessageInChat(jid) // implement this on your end
await sock.chatModify({
        delete: true,
        lastMessages: [
            {
                key: lastMsgInChat.key,
                messageTimestamp: lastMsgInChat.messageTimestamp
            }
        ]
    },
    jid
)
```
### Pin/Unpin a Chat
```javascript
await sock.chatModify({
        pin: true // or `false` to unpin
    },
    jid
)
```
### Star/Unstar a Message
```javascript
await sock.chatModify({
        star: {
            messages: [
                {
                    id: 'messageID',
                    fromMe: true // or `false`
                }
            ],
            star: true // - true: Star Message; false: Unstar Message
        }
    },
    jid
)
```

### Disappearing Messages

- Ephemeral can be:

| Time  | Seconds        |
|-------|----------------|
| Remove | 0          |
| 24h    | 86.400     |
| 7d     | 604.800    |
| 90d    | 7.776.000  |

- You need to pass in **Seconds**, default is 7 days

```javascript
// turn on disappearing messages
await sock.sendMessage(
    jid, 
    // this is 1 week in seconds -- how long you want messages to appear for
    { disappearingMessagesInChat: WA_DEFAULT_EPHEMERAL }
)

// will send as a disappearing message
await sock.sendMessage(jid, { text: 'hello' }, { ephemeralExpiration: WA_DEFAULT_EPHEMERAL })

// turn off disappearing messages
await sock.sendMessage(
    jid, 
    { disappearingMessagesInChat: false }
)
```

## User Querys

### Check If ID Exists in Whatsapp
```javascript
const [result] = await sock.onWhatsApp(jid)
if (result.exists) console.log (`${jid} exists on WhatsApp, as jid: ${result.jid}`)
```

### Query Chat History (groups too)

- You need to have oldest message in chat
```javascript
const msg = await getOldestMessageInChat(jid) // implement this on your end
await sock.fetchMessageHistory(
    50, //quantity (max: 50 per query)
    msg.key,
    msg.messageTimestamp
)
```
- Messages will be received in `messaging.history-set` event

### Fetch Status
```javascript
const status = await sock.fetchStatus(jid)
console.log('status: ' + status)
```

### Fetch Profile Picture (groups too)
- To get the display picture of some person/group
```javascript
// for low res picture
const ppUrl = await sock.profilePictureUrl(jid)
console.log(ppUrl)

// for high res picture
const ppUrl = await sock.profilePictureUrl(jid, 'image')
```

### Fetch Bussines Profile (such as description or category)
```javascript
const profile = await sock.getBusinessProfile(jid)
console.log('business description: ' + profile.description + ', category: ' + profile.category)
```

### Fetch Someone's Presence (if they're typing or online)
```javascript
// the presence update is fetched and called here
sock.ev.on('presence.update', console.log)

// request updates for a chat
await sock.presenceSubscribe(jid) 
```

## Change Profile

### Change Profile Status
```javascript
await sock.updateProfileStatus('Hello World!')
```
### Change Profile Name
```javascript
await sock.updateProfileName('My name')
```
### Change Display Picture (groups too)
- To change your display picture or a group's

```javascript
await sock.updateProfilePicture(jid, { url: './new-profile-picture.jpeg' })
```
### Remove display picture (groups too)
```javascript
await sock.removeProfilePicture(jid)
```

## Groups

- To change group properties you need to be admin

### Create a Group
```javascript
// title & participants
const group = await sock.groupCreate('My Fab Group', ['1234@s.whatsapp.net', '4564@s.whatsapp.net'])
console.log('created group with id: ' + group.gid)
await sock.sendMessage(group.id, { text: 'hello there' }) // say hello to everyone on the group
```
### Add/Remove or Demote/Promote
```javascript
// id & people to add to the group (will throw error if it fails)
await sock.groupParticipantsUpdate(
    jid, 
    ['1234567890@s.whatsapp.net', '0987654321@s.whatsapp.net'],
    'add' // replace this parameter with 'remove' or 'demote' or 'promote'
)
```
### Change Subject (name)
```javascript
await sock.groupUpdateSubject(jid, 'New Subject!')
```
### Change Description
```javascript
await sock.groupUpdateDescription(jid, 'New Description!')
```
### Change Settings
```javascript
// only allow admins to send messages
await sock.groupSettingUpdate(jid, 'announcement')
// allow everyone to send messages
await sock.groupSettingUpdate(jid, 'not_announcement')
// allow everyone to modify the group's settings -- like display picture etc.
await sock.groupSettingUpdate(jid, 'unlocked')
// only allow admins to modify the group's settings
await sock.groupSettingUpdate(jid, 'locked')
```
### Leave a Group
```javascript
// will throw error if it fails
await sock.groupLeave(jid)
```
### Get Invite Code
- To create link with code use `'https://chat.whatsapp.com/' + code`
```javascript
const code = await sock.groupInviteCode(jid)
console.log('group code: ' + code)
```
### Revoke Invite Code
```javascript
const code = await sock.groupRevokeInvite(jid)
console.log('New group code: ' + code)
```
### Join Using Invitation Code
- Code can't have `https://chat.whatsapp.com/`, only code
```javascript
const response = await sock.groupAcceptInvite(code)
console.log('joined to: ' + response)
```
### Get Group Info by Invite Code
```javascript
const response = await sock.groupGetInviteInfo(code)
console.log('group information: ' + response)
```
### Query Metadata (participants, name, description...)
```javascript
const metadata = await sock.groupMetadata(jid) 
console.log(metadata.id + ', title: ' + metadata.subject + ', description: ' + metadata.desc)
```
### Join using `groupInviteMessage`
```javascript
const response = await sock.groupAcceptInviteV4(jid, groupInviteMessage)
console.log('joined to: ' + response)
```
### Get Request Join List
```javascript
const response = await sock.groupRequestParticipantsList(jid)
console.log(response)
```
### Approve/Reject Request Join
```javascript
const response = await sock.groupRequestParticipantsUpdate(
    jid, // group id
    ['1234567890@s.whatsapp.net', '0987654321@s.whatsapp.net'],
    'approve' // or 'reject' 
)
console.log(response)
```
### Get All Participating Groups Metadata
```javascript
const response = await sock.groupFetchAllParticipating()
console.log(response)
```
### Toggle Ephemeral

- Ephemeral can be:

| Time  | Seconds        |
|-------|----------------|
| Remove | 0          |
| 24h    | 86.400     |
| 7d     | 604.800    |
| 90d    | 7.776.000  |

```javascript
await sock.groupToggleEphemeral(jid, 86400)
```

### Change Add Mode
```javascript
await sock.groupMemberAddMode(
    jid,
    'all_member_add' // or 'admin_add'
)
```

## Newsletter

- To perform most actions you must be the newsletter creator or admin

### Subscribe to Newsletter Updates
```javascript
await sock.subscribeNewsletterUpdates(jid)
```

### Update Reaction Mode
```javascript
await sock.newsletterReactionMode(jid, mode) // e.g., 'enabled', 'disabled'
```

### Update Newsletter Description
```javascript
await sock.newsletterUpdateDescription(jid, 'Your new description')
```

### Update Newsletter Name
```javascript
await sock.newsletterUpdateName(jid, 'New Name')
```

### Update Newsletter Picture
```javascript
await sock.newsletterUpdatePicture(jid, content)
```

### Remove Newsletter Picture
```javascript
await sock.newsletterRemovePicture(jid)
```

### Follow a Newsletter
```javascript
await sock.newsletterFollow(jid)
```

### Unfollow a Newsletter
```javascript
await sock.newsletterUnfollow(jid)
```

### Mute a Newsletter
```javascript
await sock.newsletterMute(jid)
```

### Unmute a Newsletter
```javascript
await sock.newsletterUnmute(jid)
```

### Perform a Newsletter Action (Generic)
```javascript
await sock.newsletterAction(jid, 'mute' | 'unmute' | 'follow' | 'unfollow')
```

### Create a Newsletter
```javascript
const newsletter = await sock.newsletterCreate('Newsletter Name', 'Newsletter Description')
console.log(newsletter)
```

### Fetch Newsletter Metadata
```javascript
// https://whatsapp.com/channel/$key
const metadata = await sock.newsletterMetadata(type, key, role) // type: 'invite' or 'direct', role: 'GUEST', etc.
```

### Get Admin Count
```javascript
const count = await sock.newsletterAdminCount(jid)
```

### Change Newsletter Owner
```javascript
/**user is Lid, not Jid */
await sock.newsletterChangeOwner(jid, userLid)
```

### Demote Newsletter Admin
```javascript
/**user is Lid, not Jid */
await sock.newsletterDemote(jid, userLid)
```

### Delete Newsletter
```javascript
await sock.newsletterDelete(jid)
```

### React to a Message in Newsletter
```javascript
// Copying the message link in the Newsletter will get
// https://whatsapp.com/channel/XXXXXXXXX/$serverId

await sock.newsletterReactMessage(jid, serverId, '❤️')
// pass null or empty to remove reaction
```

### Fetch Messages in Newsletter
```javascript
const messages = await sock.newsletterFetchMessages('direct' | 'invite', key, count, after)
```

### Fetch Updates in Newsletter
```javascript
const updates = await sock.newsletterFetchUpdates(jid, count, after, since)
```

## Privacy

### Block/Unblock User
```javascript
await sock.updateBlockStatus(jid, 'block') // Block user
await sock.updateBlockStatus(jid, 'unblock') // Unblock user
```
### Get Privacy Settings
```javascript
const privacySettings = await sock.fetchPrivacySettings(true)
console.log('privacy settings: ' + privacySettings)
```
### Get BlockList
```javascript
const response = await sock.fetchBlocklist()
console.log(response)
```
### Update LastSeen Privacy
```javascript
const value = 'all' // 'contacts' | 'contact_blacklist' | 'none'
await sock.updateLastSeenPrivacy(value)
```
### Update Online Privacy
```javascript
const value = 'all' // 'match_last_seen'
await sock.updateOnlinePrivacy(value)
```
### Update Profile Picture Privacy
```javascript
const value = 'all' // 'contacts' | 'contact_blacklist' | 'none'
await sock.updateProfilePicturePrivacy(value)
```
### Update Status Privacy
```javascript
const value = 'all' // 'contacts' | 'contact_blacklist' | 'none'
await sock.updateStatusPrivacy(value)
```
### Update Read Receipts Privacy
```javascript
const value = 'all' // 'none'
await sock.updateReadReceiptsPrivacy(value)
```
### Update Groups Add Privacy
```javascript
const value = 'all' // 'contacts' | 'contact_blacklist'
await sock.updateGroupsAddPrivacy(value)
```
### Update Default Disappearing Mode

- Like [this](#disappearing-messages), ephemeral can be:

| Time  | Seconds        |
|-------|----------------|
| Remove | 0          |
| 24h    | 86.400     |
| 7d     | 604.800    |
| 90d    | 7.776.000  |

```javascript
const ephemeral = 86400 
await sock.updateDefaultDisappearingMode(ephemeral)
```

## Broadcast Lists & Stories

### Send Broadcast & Stories
- Messages can be sent to broadcasts & stories. You need to add the following message options in sendMessage, like this:
```javascript
await sock.sendMessage(
    jid,
    {
        image: {
            url: './Media/logo.png'
        },
        caption: 'hello word'
    },
    {
        backgroundColor: '#FF0000',
        font: 1,
        statusJidList: ["1234567890@s.whatsapp.net"],
        broadcast: true
    }
)
```
- `broadcast: true` enables broadcast mode
- `statusJidList`: a list of people that you can get which you need to provide, which are the people who will get this status message.

- You can send messages to broadcast lists the same way you send messages to groups & individual chats.
- Right now, WA Web does not support creating broadcast lists, but you can still delete them.
- Broadcast IDs are in the format `12345678@broadcast`
### Query a Broadcast List's Recipients & Name
```javascript
const bList = await sock.getBroadcastListInfo('1234@broadcast')
console.log (`list name: ${bList.name}, recps: ${bList.recipients}`)
```

## Writing Custom Functionality
Baileys is written with custom functionality in mind. Instead of forking the project & re-writing the internals, you can simply write your own extensions.

### Enabling Debug Level in Baileys Logs
First, enable the logging of unhandled messages from WhatsApp by setting:
```javascript
const pino = require("pino")

const sock = makeWASocket({
    logger: pino({ level: 'silent' }),
})
```
This will enable you to see all sorts of messages WhatsApp sends in the console. 

### How Whatsapp Communicate With Us

> [!TIP]
> If you want to learn whatsapp protocol, we recommend to study about Libsignal Protocol and Noise Protocol

- **Example:** Functionality to track the battery percentage of your phone. You enable logging and you'll see a message about your battery pop up in the console: 
    ```
    {
        "level": 10,
        "fromMe": false,
        "frame": {
            "tag": "ib",
            "attrs": {
                "from": "@s.whatsapp.net"
            },
            "content": [
                {
                    "tag": "edge_routing",
                    "attrs": {},
                    "content": [
                        {
                            "tag": "routing_info",
                            "attrs": {},
                            "content": {
                                "type": "Buffer",
                                "data": [8,2,8,5]
                            }
                        }
                    ]
                }
            ]
        },
        "msg":"communication"
    }
    ``` 

The `'frame'` is what the message received is, it has three components:
- `tag` -- what this frame is about (eg. message will have 'message')
- `attrs` -- a string key-value pair with some metadata (contains ID of the message usually)
- `content` -- the actual data (eg. a message node will have the actual message content in it)
- read more about this format [here](/src/WABinary/readme.md)

# License
Copyright (c) 2025 Rajeh Taher/WhiskeySockets

Licensed under the MIT License:
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

Thus, the maintainers of the project can't be held liable for any potential misuse of this project.
