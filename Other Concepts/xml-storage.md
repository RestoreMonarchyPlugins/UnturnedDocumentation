---
position: 2
title: XML Storage
authors: BlackCris
category: developers
published: true
description: Learn how to use XML files to store and retrieve player data in your Unturned plugin. This method ensures persistent storage of player information across sessions.
---

# XML Storage
In this tutorial, we will learn how to use XML files to store and retrieve player data in an Unturned plugin. This is an effective way to persist player information. Each step will be explained in detail through a simple example.

## Creating the Data Model
First, we need to create a data model to store player information. This model is similar to a game stats record.

### Defining the Player Data Class
Add a new class named `PlayerStats` to your project and define the following properties:

```csharp
using Steamworks;

public class PlayerStats
{
    public CSteamID PlayerID { get; set; }
    public string PlayerName { get; set; }
    public int Kills { get; set; }
    public int Deaths { get; set; }
    public int ZombieKills { get; set; }
    public int Headshots { get; set; }
}
```

This class represents a single record in the game stats, tracking various player combat data.

## Writing to XML File
Next, we will write a method to store player data into an XML file.

### Creating the Write Method
Add the following methods to your plugin class:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Xml.Serialization;

public static class XMLHelper
{
    /// <summary>
    /// Writes player data to an XML file
    /// </summary>
    /// <param name="playerStatsList">List of player data</param>
    public static void WriteXML(List<PlayerStats> playerStatsList)
    {
        string path = GetXMLFilePath();
        using (FileStream file = File.Create(path))
        {
            XmlSerializer writer = new XmlSerializer(typeof(List<PlayerStats>));
            writer.Serialize(file, playerStatsList);
        }
    }

    /// <summary>
    /// Gets the path of the XML file
    /// </summary>
    /// <returns>Path of the XML file</returns>
    private static string GetXMLFilePath()
    {
        string codeBase = System.Reflection.Assembly.GetExecutingAssembly().CodeBase;
        UriBuilder uri = new UriBuilder(codeBase);
        string path = Uri.UnescapeDataString(uri.Path);

        return Path.Combine(Path.GetDirectoryName(path), "GameStats", "PlayerStats.xml");
    }
}
```

This method serializes the `PlayerStats` object list into XML format and writes it to a specified file path.

## Reading from XML File
We also need a method to read player data from the XML file.

### Creating the Read Method
Add the following method to your plugin class:

```csharp
public static class XMLHelper
{
    /// <summary>
    /// Reads player data from an XML file
    /// </summary>
    /// <returns>List of player data</returns>
    public static List<PlayerStats> ReadXML()
    {
        string path = GetXMLFilePath();

        if (!File.Exists(path))
        {
            return new List<PlayerStats>();
        }

        using (StreamReader file = new StreamReader(path))
        {
            XmlSerializer reader = new XmlSerializer(typeof(List<PlayerStats>));
            
            return (List<PlayerStats>)reader.Deserialize(file);
        }
    }
}
```

This method deserializes the `PlayerStats` object list from the XML file at the specified path.

## Example Usage
Now that we have created the methods to write and read XML files, let's see how to use them in the plugin.

### Calling the Write and Read Methods
You can call these methods to initialize or update player data when your plugin loads:

```csharp
public class YourPlugin : RocketPlugin
{
    private List<PlayerStats> playerStatsList;

    protected override void Load()
    {
        playerStatsList = XMLHelper.ReadXML();
        
        // Adding sample data
        playerStatsList.add(new PlayerStats
        {
            PlayerID = new CSteamID(123456789),
            PlayerName = "PlayerOne",
            Kills = 10,
            Deaths = 5,
            ZombieKills = 20,
            Headshots = 3
        });

        // Writing sample data to XML file
        XMLHelper.WriteXML(playerStatsList);
    }
}
```

## Handling Player Death Event
Finally, let's look at an example of how to handle player death events and update the data.

### Player Death Event Handler
Add a method to handle player death events in your plugin class, with detailed comments for each step:

```csharp
/// <summary>
/// Handles the player death event
/// </summary>
/// <param name="player">The player who died</param>
/// <param name="cause">The cause of death</param>
/// <param name="limb">The limb hit</param>
/// <param name="murderer">The killer</param>
private void OnPlayerDeath(UnturnedPlayer player, EDeathCause cause, ELimb limb, CSteamID murderer)
{
    // Find the player's stats
    PlayerStats playerStats = playerStatsList.FirstOrDefault(p => p.PlayerID == player.CSteamID);
    if (playerStats == null)
    {
        // If player data doesn't exist, create a new record and add it to the list
        playerStatsList.add(new PlayerStats { PlayerID = player.CSteamID, PlayerName = player.CharacterName, Deaths = 1 });
        playerStats = playerStatsList.FirstOrDefault(p => p.PlayerID == player.CSteamID);
    }
    else
    {
        // If player data exists, update the death count
        playerStats.Deaths++;
    }
    
    // Write the updated data to the XML file
    XMLHelper.WriteXML(playerStatsList);

    // If there is a killer, and the killer is not the player themselves
    if (murderer != CSteamID.Nil && murderer != player.CSteamID)
    {
        // Find the killer's stats
        UnturnedPlayer killer = UnturnedPlayer.FromCSteamID(murderer);
        PlayerStats killerStats = playerStatsList.FirstOrDefault(p => p.PlayerID == killer.CSteamID);
        if (killerStats == null)
        {
            // If killer data doesn't exist, create a new record and add it to the list
            playerStatsList.add(new PlayerStats { PlayerID = killer.CSteamID, PlayerName = killer.CharacterName, Kills = 1 });
            killerStats = playerStatsList.FirstOrDefault(p => p.PlayerID == killer.CSteamID);
        }
        else
        {
            // If killer data exists, update the kill count
            killerStats.Kills++;
        }
        
        // Write the updated data to the XML file
        XMLHelper.WriteXML(playerStatsList);
    }
}
```

## Summary
In this tutorial, you learned how to use XML files to store and retrieve player data in an Unturned plugin. This is a simple and effective method for persistent storage, suitable for various plugins that need to save player information. We hope this article has been helpful to you. If you have any questions or need further assistance, feel free to ask on our [forum](/forum).