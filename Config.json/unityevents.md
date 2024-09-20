---
position: 3
title: UnityEvents
authors: MCrow
published: true
---

UnityEvents is a section of debug options which probably game developer uses to test some feature.

It doesn't include any interesting options, so I recommend leaving it as it is.

```json
"UnityEvents": {
  "Allow_Server_Messages": false,
  "Allow_Server_Commands": false,
  "Allow_Client_Messages": false,
  "Allow_Client_Commands": false
},
```

### Allow_Server_Messages
Allows Unity events to broadcast text chat messages from the server.
Messenger context is logged to help track down abusive assets.

### Allow_Server_Commands
Allows Unity events to execute commands from the server.  
Messenger context is logged to help track down abusive assets.

### Allow_Client_Messages
Allows Unity events to broadcast text chat messages from the client.
Basically allows the client to send messages in chat modes it usually can't send like `Welcome` or `Say`.

### Allow_Client_Commands
Allow Unity events to execute commands from the client.
The player still needs to be admin to actually execute those commands, so it's not a security risk.