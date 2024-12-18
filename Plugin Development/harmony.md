---
position: 3
title: Patch Any Code with Harmony!
authors: sunnamed
category: developers
published: true
description: Learn how to patch any code with Harmony.
---

# Introduction

Have you ever wanted to create a plugin but couldn’t do it without changing Unturned's code? Or maybe you wanted to fix someone else's plugin? There’s a solution for that! You can edit anyone's code directly from your own plugin using [Harmony](https://github.com/pardeike/Harmony).

In this guide, I'll show you how to use Harmony. At the end, you can download a free example plugin that demonstrates best practices. It makes zombies ignore you completely when you're vanished!

# Harmony Documentation

The [Harmony Docs](https://harmony.pardeike.net/articles/intro.html) are your best friend. They explain most things you’ll need. I also recommend searching for examples of Harmony code directly on [GitHub](https://github.com/) (use "harmony" as the search term). This often gives better results than using Google or other search engines.

# Getting Started

First, download [Lib.Harmony](https://www.nuget.org/packages/Lib.Harmony) for your project. Alternatively, you can download the `.dll` file from [GitHub Releases](https://github.com/pardeike/Harmony/releases) and reference it manually. I recommend using version `2.2.2`, as it's widely supported by plugins.

## Steps

### 1. Initialize Harmony

Create a unique Harmony ID (like `"com.yourplugin.name"`) and initialize Harmony in your plugin:

```csharp
class Plugin : RocketPlugin
{
    const string HarmonyId = "com.yourplugin.name";
    Harmony harmony;

    protected override void Load()
    {
        harmony = new Harmony(HarmonyId);
        harmony.PatchAll(); // This applies patches from your plugin.
    }
}
```

### 2. Unpatch Harmony

To avoid issues like double patches when your plugin unloads or crashes, unpatch Harmony in the `Unload()` method. Always pass your Harmony ID to `UnpatchAll()` to ensure it only unpatches your patches.

```csharp
protected override void Unload()
{
    if (harmony != null)
    {
        try
        {
            harmony.UnpatchAll(HarmonyId); // Unpatch only your patches.
        }
        catch
        {
            // Ignore errors
        }
    }
}
```

### 3. Create a Patch

Create a folder (e.g., `Patches`) and add a class, like `Patch`. When you call `harmony.PatchAll()`, Harmony automatically finds all patches in your plugin.

Here’s an example of making animals stop attacking players. We'll patch the `sendAnimalAttack` method in `AnimalManager`:

#### Original Code (from dnSpy or Unturned-Datamining)

```csharp
public class AnimalManager
{
    public static void sendAnimalAttack(Animal animal)
    {
        AnimalManager.SendAnimalAttack.InvokeAndLoopback(
            ENetReliability.Unreliable, 
            Provider.GatherRemoteClientConnections(), 
            animal.index);
    }
}
```

#### Prefix Patch Example

Use a `HarmonyPrefix` to prevent the original method from running:

```csharp
[HarmonyPatch]
class Patch
{
    [HarmonyPatch(typeof(AnimalManager), nameof(AnimalManager.sendAnimalAttack))]
    [HarmonyPrefix]
    static bool PrefixAttack(Animal animal)
    {
        // Return false to skip Unturned's code, true to allow it.
        return false;
    }
}
```

#### Access the Original Instance

To access the `AnimalManager` instance in your patch:

```csharp
static bool PrefixAttack(AnimalManager __instance, Animal animal)
{
    // Use __instance for the AnimalManager instance
    return false;
}
```

#### Postfix Patch Example

To run your code after the original method:

```csharp
[HarmonyPatch(typeof(AnimalManager), nameof(AnimalManager.sendAnimalAttack))]
[HarmonyPostfix]
static void PostfixAttack(Animal animal)
{
    // Your code here, runs after the original method.
}
```

# Next Steps

Here are some topics to explore next:

- **Unload your plugin if patches fail.**  
- **Learn Harmony Transpilers:** Replace or modify code in a method. ([Harmony Transpiler Guide](https://harmony.pardeike.net/articles/patching-transpiler.html))  
- **Learn IL Code Basics:** Helps with advanced patches like Transpilers. ([Read about IL Code](https://en.wikipedia.org/wiki/Common_Intermediate_Language))  
- **Handle Harmony Exceptions:** Show Harmony errors in logs.  
- **Protect against bad unpatching:** Patch `UnpatchAll()` to prevent other plugins from breaking your patches.  

# Real Plugin Example

Here’s the example plugin I mentioned earlier. It fixes an issue where zombies can still detect vanished players. 

Check out the [source code here](https://github.com/sunnamed-Unturned/BetterVanish). Feel free to modify and try it on your server!

# Conclusion

You’ve learned the basics of Harmony! Whether you're a beginner or an experienced developer, I hope you found something useful here. See you next time!