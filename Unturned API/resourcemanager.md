---
position: 2
title: Resource Manager
authors: ArBajt, MCrow
category: developers
published: true
description: List of events that be used in Rocket plugins from ResourceManager class.
---
# Unturned ResourceManager Events
List of events that be used in Rocket plugins from [ResourceManager](https://github.com/Unturned-Datamining/Unturned-Datamining/blob/linux-client-preview/Assembly-CSharp/SDG.Unturned/ResourceManager.cs) class.

## Important Notes
- Always subscribe to events in `Load()` method
- Always unsubscribe in `Unload()` to prevent memory leaks
- Event handlers must match the exact signature of the event
- Use `ref` parameters when required by the event signature

## Quick Example
```csharp
public class ExamplePlugin : RocketPlugin<ExampleConfiguration>
{
    protected override void Load()
    {
        ResourceManager.onDamageResourceRequested += HandlePlayerOnDamageResourceRequested;
    }

    protected override void Unload()
    {
        ResourceManager.onDamageResourceRequested -= HandlePlayerOnDamageResourceRequested;
    }

    private void HandlePlayerOnDamageResourceRequested(CSteamID instigatorSteamID, Transform objectTransform, ref ushort pendingTotalDamage, ref bool shouldAllow, EDamageOrigin damageOrigin)
    {
        // Convert the CSteamID to an UnturnedPlayer class variable
        UnturnedPlayer unturnedPlayer = UnturnedPlayer.FromCSteamID(instigatorSteamID);

        // Set the amount of damage that will be dealt to the resource
        pendingTotalDamage = 25;

        // Send a message to the player showing how much damage was dealt and the name of the resource object
        UnturnedChat.Say(unturnedPlayer, $"You dealt {pendingTotalDamage} damage to a resource with name: '{objectTransform.name}'", Color.green);
    }
}
```

## onDamageResourceRequested
This event is called when resource takes damage (from player or zombie for example).
```csharp
private void HandlePlayerOnDamageResourceRequested2(CSteamID instigatorSteamID, Transform objectTransform, ref ushort pendingTotalDamage, ref bool shouldAllow, EDamageOrigin damageOrigin)
{
    // Convert the CSteamID to an UnturnedPlayer class variable
    UnturnedPlayer unturnedPlayer = UnturnedPlayer.FromCSteamID(instigatorSteamID);

    // If the damage origin is charge, block the damage and notify the player
    if (damageOrigin == EDamageOrigin.Charge_Explosion)
    {
        shouldAllow = false; // block damage from Charge
        UnturnedChat.Say(unturnedPlayer, "You cannot damage resources with demolition charges!", Color.red);
    }
    else
    {
        shouldAllow = true; // Allow damage
        UnturnedChat.Say(unturnedPlayer, $"You dealt damage to a resource named '{objectTransform.name}'.", Color.green);
    }
}
```
