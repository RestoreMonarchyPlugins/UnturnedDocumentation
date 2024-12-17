---
position: 3
title: Unturned Player
authors: MCrow
category: developers
published: true
description: The UnturnedPlayer class is a wrapper around SDG.Unturned.Player that provides additional functionality and integrates with the Rocket framework.
---
# Unturned Player

The [UnturnedPlayer](https://github.com/SmartlyDressedGames/Legally-Distinct-Missile/blob/master/Rocket.Unturned/Player/UnturnedPlayer.cs) class is a wrapper around **SDG.Unturned.Player** that provides additional functionality and integrates with the Rocket framework. It implements **IRocketPlayer** for use with Rocket's permission system and commands.

## Common Usage Examples

### Converting Players
Remember that you can get instance of UnturnedPlayer only from players that are currently connected to the server.
```csharp
// From SDG.Unturned.Player
SDG.Unturned.Player nativePlayer = /* ... */;
UnturnedPlayer player = UnturnedPlayer.FromPlayer(nativePlayer);

// From SteamPlayer
SteamPlayer steamPlayer = /* ... */;
UnturnedPlayer player = UnturnedPlayer.FromSteamPlayer(steamPlayer);

// From CSteamID
CSteamID steamId = /* ... */;
UnturnedPlayer player = UnturnedPlayer.FromCSteamID(steamId);

// From player name or Steam64ID
UnturnedPlayer player = UnturnedPlayer.FromName("playerName");
UnturnedPlayer player = UnturnedPlayer.FromName("76561198012345678");
```

### Usage in Commands

```csharp
public class ExampleCommand : IRocketCommand
{
    public AllowedCaller AllowedCaller => AllowedCaller.Player;

    public void Execute(IRocketPlayer caller, string[] args)
    {
        // You can safely cast IRocketPlayer to UnturnedPlayer, because AllowedCaller is Player
        UnturnedPlayer player = (UnturnedPlayer)caller;

        unturnedPlayer.GodMode = true;
        unturnedPlayer.MaxSkills();
    }
}
```

## Properties

### Basic Information
- `CSteamID` - Player's Steam ID
- `Id` - Steam64 ID as string (implements IRocketPlayer)
- `DisplayName` - Character name (implements IRocketPlayer)
- `CharacterName` - In-game character name
- `SteamName` - Steam profile name
- `IP` - Player's IP address
- `IsAdmin` - Whether player has admin status
- `IsPro` - Whether player has Unturned Gold DLC
- `Ping` - Player's current ping

### Player State
- `Health` - Current health (0-100)
- `Hunger` - Current food level (0-100)
- `Thirst` - Current water level (0-100)
- `Infection` - Current virus/infection level (0-100)
- `Stamina` - Current stamina level (0-100)
- `Experience` - Player's experience points
- `Reputation` - Player's reputation points
- `Dead` - Whether player is dead
- `Bleeding` - Get/set bleeding status
- `Broken` - Get/set broken bones status

### Position & Movement
- `Position` - Current world position (Vector3)
- `Rotation` - Current Y-axis rotation
- `Stance` - Current player stance (standing/crouching/prone)
- `CurrentVehicle` - Vehicle player is in (null if not in vehicle)
- `IsInVehicle` - Whether player is in a vehicle

### Methods

### Item Management
```csharp
// Give item by ID
player.GiveItem(289, 1); // Gives 1 Maple Gun

// Give vehicle
player.GiveVehicle(57); // Gives Police Car

// Access inventory
PlayerInventory inventory = player.Inventory;
```

### Player State Modification
```csharp
// Heal player
player.Heal(100); // Full heal
player.Heal(50, false, true); // Heal 50, stop bleeding, keep broken

// Damage player
player.Damage(25, Vector3.zero, EDeathCause.ZOMBIE, ELimb.SPINE, CSteamID.Nil);

// Set states
player.Bleeding = true;
player.Broken = false;
player.GodMode = true;
player.VanishMode = true;

// Kill player
player.Suicide();
```

### Teleportation
```csharp
// Teleport to coordinates
player.Teleport(new Vector3(100, 100, 100), 90f);

// Teleport to another player
UnturnedPlayer target = /* ... */;
player.Teleport(target);

// Teleport to location node
player.Teleport("seattle"); // Teleports to node containing "seattle"
```

### Skills & Experience
```csharp
// Set experience
player.Experience = 1000;

// Max all skills
player.MaxSkills();

// Modify specific skill
UnturnedSkill skill = new UnturnedSkill(2, 4); // Speciality 2, Skill 4
player.SetSkillLevel(skill, 5);
byte level = player.GetSkillLevel(skill);
```

### Admin Actions
```csharp
// Kick player
player.Kick("Reason for kick");

// Ban player
player.Ban("Reason for ban", 3600); // Ban for 1 hour
player.Ban(instigatorSteamId, "Reason", 0); // Permanent ban with instigator

// Set admin status
player.Admin(true); // Give admin
player.Admin(false); // Remove admin
player.Admin(true, instigatorPlayer); // Give admin with instigator
```

### Visual Effects
```csharp
// Trigger effect on player
player.TriggerEffect(123); // Effect ID 123
```

### Steam Information
Using Steam profile information in the game thread can cause lag, because it's fetched from the Steam API.  
**Executing it on the main thread will cause LAG!**

```csharp
// Get Steam profile
// Since it's being fetched, make sure to assign it to a variable if you need to use it multiple times.
Profile steamProfile = player.SteamProfile;

string avatarUrl = steamProfile.AvatarFull.ToString();
bool isVacBanned = steamProfile?.IsVacBanned ?? false;
bool isLimitedAccount = steamProfile?.IsLimitedAccount ?? false;
```

## Components

You can access Unity components on the player using GetComponent:

```csharp
PlayerLife life = player.GetComponent<PlayerLife>();
```

## Notes
- Always validate IRocketPlayer is UnturnedPlayer before casting in commands
- The Player property provides access to the underlying SDG.Unturned.Player if needed
- Many methods have multiple overloads for different use cases
- Some properties are read-only while others can be modified