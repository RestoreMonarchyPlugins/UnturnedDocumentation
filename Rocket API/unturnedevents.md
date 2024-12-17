---
position: 1
title: Unturned Events
authors: MCrow
category: developers
published: true
description: List of events that be used in Rocket plugins from UnturnedEvents class.
---
# Unturned Events
## Important Notes
- Always subscribe to events in `Load()` method
- Always unsubscribe in `Unload()` to prevent memory leaks
- Event handlers must match the exact signature of the event
- Use `ref` parameters when required by the event signature

## Quick Example
```csharp
public class ExamplePlugin : RocketPlugin<ExampleConfiguration>
{
    private void HandleConnect(UnturnedPlayer player)
    {
        Logger.Log($"{player.DisplayName} connected!");
    }

    protected override void Load()
    {
        U.Events.OnPlayerConnected += HandleConnect;
    }

    protected override void Unload()
    {
        U.Events.OnPlayerConnected -= HandleConnect;
    }
}
```


## OnPlayerConnected
Called when player completes connection.
```csharp
private void HandlePlayerConnect(UnturnedPlayer player)
{
   UnturnedChat.Say(player, "Welcome to the server!");
}
```

## OnBeforePlayerConnected
Called before player completes connection.
```csharp
private void HandleBeforeConnect(UnturnedPlayer player)
{
    Logger.Log($"Player {player.CharacterName} is connecting...");
}
```

## OnPlayerDisconnected
Called when player leaves server.
```csharp
private void HandleDisconnect(UnturnedPlayer player)
{
    Logger.Log($"{player.DisplayName} left the server");
}
```

## OnPlayerDamaged
Called when player takes damage.
```csharp
private void HandleDamage(UnturnedPlayer player, ref EDeathCause cause, 
    ref ELimb limb, ref UnturnedPlayer killer, ref Vector3 direction, 
    ref float damage, ref float times, ref bool canDamage)
{
    if (killer != null)
    {
        float totalDamage = damage * times;
        Logger.Log($"{killer.DisplayName} hit {player.DisplayName} for {totalDamage} damage!");
    }
}
```

## OnShutdown
Called when server begins shutdown.
```csharp
private void HandleShutdown()
{
    Logger.Log("Server is shutting down!");
}
```
