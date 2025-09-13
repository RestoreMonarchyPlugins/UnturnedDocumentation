---
position: 5
title: Config.txt
authors: MCrow
category: servers
published: true
description: The Config.txt file allows you to configure various settings for your Unturned server, such as gameplay options, performance settings, and more.
---

# Config.txt
The **3.25.8.0** update introduced a new configuration file called `Config.txt` that replaced old `Config.json` file. This file allows you to configure various settings for your Unturned server, such as gameplay options, performance settings, and more.

## Where is Config.txt located?
You'll find this file in the root directory of your server. For example, it might be located at:
```
Servers/unturned/Config.txt
```

## What does Config.txt look like?
The `Config.txt` file is a plain text file that uses a simple key-value pair format. Each setting is defined on a new line, with the key and value separated by an a single space (` `). Comments can be added using the `//` symbol.

The file contains descriptions for each setting, making it easy to understand what each option does. For example:
```json
Browser
{
	// > URL of a 64x64 image shown in the upper-left of the server lobby menu.
	Icon

	// > URL of a 32x32 image shown in the server list.
	Thumbnail

	// > Short description underneath the server name in the server lobby menu.
	Desc_Hint
```

to set the server icon, you would modify the `Icon` setting like this:
```
Icon https://i.imgur.com/3dCsuu2.png
```

The format is user-friendly and easy to edit. In case you make a mistake, you're gonna see a warning in the server console when you start your server and the server will use the default value for that setting.

The default values of some settings may differ between Easy, Normal, and Hard modes. You can change the mode in the `Commands.dat` file using the `mode` command.

To learn more about the Config.txt file we recommend checking out the [official documentation](https://docs.smartlydressedgames.com/en/latest/servers/server-configuration.html).