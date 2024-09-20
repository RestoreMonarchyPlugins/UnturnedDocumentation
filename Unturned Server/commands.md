---
position: 4
title: Commands
authors: MCrow
published: true
---
### Admin
The `admin` command is used to give a player admin blue hammer. Admin can use all commands and has access to all permissions.

**Syntax:** `/admin <player>`
- `<player>`: The name of the player to give admin to.

**Example:** `/admin MCrow`

### Unadmin
The `unadmin` command is used to remove admin blue hammer from a player.

**Syntax:** `/unadmin <player>`
- `<player>`: The name of the player to remove admin from.

**Example:** `/unadmin MCrow`

> **💡 PRO TIP**  
> Unfortunately there is no way to remove admin from someone who is offline. If you want you can delete all admins.  
Stop the server and delete the `Adminlist.dat` file which you will find in the same directory as `Commands.dat`.

### Ban
The `ban` command is used to ban a player from the server for a specified time.  

**Syntax:** `/ban <player> [reason] [time]`
- `<player>`: The name or Steam ID of the player to ban.
- `[reason]`: The reason for the ban.
- `[time]`: The duration of the ban in seconds. If not specified, the ban is permanent.

**Example:** `/ban MCrow "Griefing and cross-teaming" 3600`

> **💡 PRO TIP**  
> Remember to use the quotation marks for the reason if it has more than one word.

### Unban
The `unban` command is used to unban a player from the server. 

**Syntax:** `/unban <player>`
- `<player>`: The Steam ID of the player to unban.

**Example:** `/unban 76561198285897058`

> **💡 PRO TIP**  
> If you want to delete all bans from the server, stop the server and delete the `Blacklist.dat` file in the same directory as `Commands.dat`.

### Kick
The `kick` command is used to kick a player from the server.

**Syntax:** `/kick <player> [reason]`
- `<player>`: The name of the player to kick.
- `[reason]`: The reason for the kick.

**Example:** `/kick MCrow "Abusive language"`

### Spy
The `spy` command is used to take a screenshot of a player's screen. After using the command, you can view when you press the `ESC` key.

**Syntax:** `/spy <player>`
- `<player>`: The name of the player to spy on.

**Example:** `/spy MCrow`

### Day
The `day` command is used to change the time of the server to day.

**Syntax:** `/day`

### Night
The `night` command is used to change the time of the server to night.

**Syntax:** `/night`

### Weather
The `weather` command is used to change the weather of the server.

**Syntax:** `/weather <weather>`
- `<weather>`: The weather to set. Possible values are `none`, `disable`, `storm`, `blizzard`

### Time
The `time` command is used to change the time of the server.

**Syntax:** `/time <time>`
- `<time>`: The time to set in seconds. By default full day in Unturned is 1 hour, so 3,600 seconds.

**Example:** `/time 3200`

### Airdrop
The `airdrop` command is used to call an airdrop.

**Syntax:** `/airdrop`

### Say
The `say` command is used to send a message to all players on the server.

**Syntax:** `/say <message> [r] [g] [b]`

**Example:** `/say Welcome to our server!`
