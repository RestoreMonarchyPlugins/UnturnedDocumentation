---
position: 3
title: Unturned Player Events
authors: MCrow
category: developers
published: true
description: List of events that can be used in Rocket plugins from the UnturnedPlayerEvents class. These events track individual player state changes and actions.
---
# UnturnedPlayerEvents
List of events that can be used in Rocket plugins from the [UnturnedPlayerEvents](https://github.com/SmartlyDressedGames/Legally-Distinct-Missile/blob/master/Rocket.Unturned/Events/UnturnedPlayerEvents.cs) class. These events track individual player state changes and actions.

## Important Notes
- Events can be subscribed using the static format: `UnturnedPlayerEvents.EventName += HandlerMethod`
- Always unsubscribe from events in `Unload()` to prevent memory leaks
- Event handlers must match the exact signature of the event
- Use `ref` parameters when required by the event signature
- `using Rocket.Unturned.Events;` is required

## Quick Example
```csharp
public class ExamplePlugin : RocketPlugin<ExampleConfiguration>
{
    protected override void Load()
    {
        UnturnedPlayerEvents.OnPlayerRevive += OnPlayerRevive;
    }

    protected override void Unload()
    {
        UnturnedPlayerEvents.OnPlayerRevive -= OnPlayerRevive;
    }

    private void OnPlayerRevive(UnturnedPlayer player, Vector3 position, byte angle)
    {
        UnturnedChat.Say(player, "Welcome back to life!");
    }
}
```

## OnPlayerChatted
Called when a player sends a chat message. Allows modifying the message color or canceling the message.
```csharp
private void OnPlayerChatted(UnturnedPlayer player, ref Color color, string message, EChatMode chatMode, ref bool cancel)
{
    if (message.Contains("bad word"))
    {
        cancel = true; // Prevent message from being sent
    }
    color = Color.green; // Change message color
}
```

## OnPlayerUpdateHealth
Called when a player's health value changes.
```csharp
private void OnPlayerUpdateHealth(UnturnedPlayer player, byte health)
{
    if (health < 30)
    {
        UnturnedChat.Say(player, "Warning: Low health!");
    }
}
```

## OnPlayerUpdateFood
Called when a player's food value changes.
```csharp
private void OnPlayerUpdateFood(UnturnedPlayer player, byte food)
{
    if (food < 20)
    {
        UnturnedChat.Say(player, "You're getting hungry!");
    }
}
```

## OnPlayerUpdateWater
Called when a player's water value changes.
```csharp
private void OnPlayerUpdateWater(UnturnedPlayer player, byte water)
{
    if (water < 20)
    {
        UnturnedChat.Say(player, "You're getting thirsty!");
    }
}
```

## OnPlayerUpdateVirus
Called when a player's virus level changes.
```csharp
private void OnPlayerUpdateVirus(UnturnedPlayer player, byte virus)
{
    if (virus > 80)
    {
        UnturnedChat.Say(player, "Warning: High infection!");
    }
}
```

## OnPlayerUpdateBleeding
Called when a player's bleeding status changes.
```csharp
private void OnPlayerUpdateBleeding(UnturnedPlayer player, bool bleeding)
{
    if (bleeding)
    {
        UnturnedChat.Say(player, "You are bleeding!");
    }
}
```

## OnPlayerUpdateBroken
Called when a player's broken bone status changes.
```csharp
private void OnPlayerUpdateBroken(UnturnedPlayer player, bool broken)
{
    if (broken)
    {
        UnturnedChat.Say(player, "You have a broken bone!");
    }
}
```

## OnPlayerDeath
Called when a player dies.
```csharp
private void OnPlayerDeath(UnturnedPlayer player, EDeathCause cause, 
    ELimb limb, CSteamID murderer)
{
    Logger.Log($"{player.DisplayName} died from {cause}");
    if (murderer != CSteamID.Nil)
    {
        Logger.Log($"Killed by: {murderer}");
    }
}
```

## OnPlayerDead
Called after a player dies, with additional position information.
```csharp
private void OnPlayerDead(UnturnedPlayer player, Vector3 position)
{
    Logger.Log($"{player.DisplayName} died at position: {position}");
}
```

## OnPlayerRevive
Called when a player respawns.
```csharp
private void OnPlayerRevive(UnturnedPlayer player, Vector3 position, byte angle)
{
    UnturnedChat.Say(player, "Welcome back to life!");
}
```

## OnPlayerInventoryAdded
Called when an item is added to player's inventory.
```csharp
private void OnPlayerInventoryAdded(UnturnedPlayer player, InventoryGroup group, byte index, ItemJar item)
{
    Logger.Log($"{player.DisplayName} picked up item: {item.item.id}");
}
```

## OnPlayerInventoryRemoved
Called when an item is removed from player's inventory.
```csharp
private void OnPlayerInventoryRemoved(UnturnedPlayer player, InventoryGroup group, byte index, ItemJar item)
{
    Logger.Log($"{player.DisplayName} removed item: {item.item.id}");
}
```

## OnPlayerInventoryUpdated
Called when an item in the player's inventory is updated.
```csharp
private void OnPlayerInventoryUpdated(UnturnedPlayer player, InventoryGroup group, byte index, ItemJar item)
{
    Logger.Log($"{player.DisplayName}'s inventory updated at index {index}");
}
```

## OnPlayerInventoryResized
Called when a player's inventory size changes.
```csharp
private void OnPlayerInventoryResized(UnturnedPlayer player, InventoryGroup group, byte oldSize, byte newSize)
{
    Logger.Log($"{player.DisplayName}'s {group} inventory resized from {oldSize} to {newSize}");
}
```

## OnPlayerUpdateExperience
Called when a player's experience changes.
```csharp
private void OnPlayerUpdateExperience(UnturnedPlayer player, uint experience)
{
    Logger.Log($"{player.DisplayName}'s experience: {experience}");
}
```

## OnPlayerUpdateStat
Called when any player stat is incremented.
```csharp
private void OnPlayerUpdateStat(UnturnedPlayer player, EPlayerStat stat)
{
    Logger.Log($"{player.DisplayName}'s stat {stat} increased");
}
```

## OnPlayerUpdatePosition
Called when a player's position changes.
```csharp
private void OnPlayerUpdatePosition(UnturnedPlayer player, Vector3 position)
{
    Logger.Log($"{player.DisplayName} moved to: {position}");
}
```

## OnPlayerUpdateStance
Called when a player's stance changes (standing, crouching, prone).
```csharp
private void OnPlayerUpdateStance(UnturnedPlayer player, byte stance)
{
    Logger.Log($"{player.DisplayName}'s stance changed to: {stance}");
}
```

## OnPlayerUpdateGesture
Called when a player performs a gesture (wave, point, etc.).
```csharp
private void OnPlayerUpdateGesture(UnturnedPlayer player, PlayerGesture gesture)
{
    Logger.Log($"{player.DisplayName} performed gesture: {gesture}");
}
```

## OnPlayerUpdateStamina
Called when a player's stamina value changes.
```csharp
private void OnPlayerUpdateStamina(UnturnedPlayer player, byte stamina)
{
    if (stamina < 10)
    {
        UnturnedChat.Say(player, "You're getting tired!");
    }
}
```

## OnPlayerWear
Called when a player equips or changes wearable items (clothing).
```csharp
private void OnPlayerWear(UnturnedPlayer player, Wearables wear, ushort id, byte? quality)
{
    Logger.Log($"{player.DisplayName} equipped {wear} with ID {id}");
}
```