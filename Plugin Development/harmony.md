---
position: 3
title: Harmony Patching
authors: sunnamed
category: developers
published: false
description: Learn how to use Harmony to patch methods in Unturned plugins.
---

# Harmony Patching
Harmony patching in Unturned plugins is mostly used to modify the game's code or add hooks to methods for which events are not available. 

[Harmony](https://github.com/pardeike/Harmony) is a library that allows you to patch methods in assemblies at runtime. It's used in many plugins to modify the game's behavior without changing the game's code directly.

In this guide, we will show the basic usage of using Harmony in Unturned plugins.

## Requirements
Harmony is a .NET library, so you first need to add it to your project. You can do this by installing the `Lib.Harmony` NuGet package.

![Lib.Harmony in NuGet Package Manager](assets/harmony_nuget.png)

> **💡 PRO TIP**  
> Remember to copy `0Harmony.dll` to `Rocket/Libraries` of your server and to always share it with your plugin.  
> After building the project, the `0Harmony.dll` will be in the same `bin/Debug` or `bin/Release` directory as your plugin dll.
> 
> ![0Harmony.dll in File Explorer](assets/harmony_dll.png)

## Applying Patches
To apply patches that are in your plugin, you need to create a Harmony instance with a unique ID. This ID is used to identify your patches and avoid conflicts with other plugins.

I suggest the following format for the ID: `com.brand_name.plugin_name`. For example, `com.restoremonarchy.upets` or `com.sunnamed.bettervanish`.

Below is an example of how to apply patches in your plugin:
```cs
using Rocket.Core.Plugins;
using HarmonyLib;

public class UPets : RocketPlugin
{
    public const string HarmonyId = "com.restoremonarchy.upets";
    private Harmony harmony;

    protected override void Load()
    {
        harmony = new Harmony(HarmonyId);
        harmony.PatchAll(); // Applies all patches in your plugin.
    }

    protected override void Unload()
    {
        if (harmony != null)
        {
            harmony.UnpatchAll(HarmonyId);
        }
    }
}
```

## Where to Find Methods to Patch?
If you don't know yet what method you want to patch, you can look for it in Unturned code. The easiest way is by using de-compilers like dnSpy or DotPeek by JetBrains. You can learn more about this method in our blog post [Finding Events](/developers/blog/unt07-finding-events).

## Creating Patches
There are many methods of using Harmony to patch methods. For the complete list, see the [Harmony documentation](https://harmony.pardeike.net/articles/patching.html). 

Here I will show you my favorite way of patching methods using Harmony. We will create a patch that will prevent animals from attacking players.

First create a new folder in your project and call it **Patches**. Inside this folder, create a new class and name it `AnimalManagerPatches` where **AnimalManager** is the class we want to patch.
```cs
using HarmonyLib;
using SDG.Unturned;

namespace RestoreMonarchy.UPets.Patches
{
    [HarmonyPatch(typeof(AnimalManager))] // Specify the target class.
    class AnimalManagerPatches
    {
        [HarmonyPatch("sendAnimalAttack")] // Specify the target method.
        [HarmonyPrefix] // The patch type. Prefix runs before the original method.
        static bool PrefixAttack(Animal animal) // You can use arguments from the original method
        {
            return false; // Return false to skip the original method.
        }
    }
}
```

In the example above, we created a patch that will run before the `sendAnimalAttack` method in the `AnimalManager` class. We return `false` to skip the original method. This way, animals like wolves and bears will not attack players.

> **💡 PRO TIP**  
> Patch methods must be static but they can be public or private.  
> You can also use `void` as the return type if you don't need to skip the original method.

## Conclusion
This is only a simple example and one way that you can use Harmony in your plugins. There are many other ways to use Harmony, and you can find more information in the [Harmony documentation](https://harmony.pardeike.net/articles/intro.html).




<!-- ## Steps

### 1. Initialize Harmony & Patches

Initialize harmony with unique name (ID) and save it to a variable `harmony = new Harmony("com.restoremonarchy.plugin")`, the unique name is `com.restoremonarchy.yourplugin`, keep in mind other plugins might make a similar ID and pass it to harmony just like you, and you both will have problems. 

My recommendations about ID naming style:
1. `com.brand_name.plugin_name` - (`com.restoremonarchy.plugin`)
2. `Name` - simply pass the Name of your plugin to Harmony (`harmony = new Harmony(Name)`)
3. Keep it as you wish.

Create instance of Harmony in your plugin:

```cs
public class YourPlugin : RocketPlugin
{
    public const string HarmonyId = "com.restoremonarchy.yourplugin";
    private Harmony harmony;

    protected override void Load()
    {
        harmony = new Harmony(HarmonyId); // We need to create an instance of Harmony with unique ID.
        harmony.PatchAll(); // This finds and applies all harmony patches in your plugin assembly.
    }
}
```

### 2. Unpatch Harmony

To avoid issues like double patches when your plugin unloads or crashes, unpatch Harmony in the plugin `Unload()` method. Always pass your Harmony ID to `UnpatchAll(..)` to ensure it only unpatches your patches.

```cs
protected override void Unload()
{
    if (harmony != null) // make sure to check if harmony is not null.
    {
        harmony.UnpatchAll(HarmonyId); // Unpatch only your patches.
    }
}
```

Alternative approach:
```cs
harmony.UnpatchAll(harmony.Id);
// You don't need to have a `const string HarmonyId = ..;` anymore.
```

### 3. Patch

#### Patch Structure

1. Have `[HarmonyPatch]` attribute on top of your patch class, this is needed to let Harmony find your patch.
2. Create a **static** method for patch `static void SomePatchName(..)`, if you don't make it `static` the patch won't be applied.
3. `[HarmonyPatch(typeof(AnimalManager), "target method name...")]` attribute on top of the `static void SomePatchName(..)` method.
4. Specify the Harmony patch type on top of your patch method, there's actually 3 of them, but we will use only 2 of them:
    - `[HarmonyPrefix]` - once `sendAnimalAttack` method is called your patch is going to be called before Unturned's code, also you can prevent the original code to be executed, and instead for example execute only your own code.
    - `[HarmonyPostfix]` - once `sendAnimalAttack` method is called your patch is going to be called after Unturned's code, in simple words at the of the method.
    - `[HarmonyTranspiler]` - it's way more complex type of patch, there you can add/replace original Unturned's code instructions with your own, for example skip `method1()` and put your own method after `method1()` at some offset. 

#### Original Code (from dnSpy or Unturned-Datamining)

That code I found using [dnSpy](https://github.com/dnSpy/dnSpy) by decompiling the `Unturned_Data\Managed\Assembly-CSharp.dll`, or you can also search for your target method or thing you want to patch on [Unturned-Datamining](https://github.com/Unturned-Datamining/Unturned-Datamining) GitHub repo.

```cs
public class AnimalManager
{
    // other code is removed...

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

Create a folder (e.g., `Patches`) and add a class, like `Patch`. When you call `harmony.PatchAll()`, Harmony automatically finds all patches in your plugin.

Here’s an example of making animals stop attacking players. We'll patch the `sendAnimalAttack` method in `AnimalManager` class.

Use a `HarmonyPrefix`, so your patch is going to be called before the original method (`AnimalManager.sendAnimalAttack`):

```cs
[HarmonyPatch] // Put this attribute on top of your patch class, that's how Harmony will find that.
class Patch
{
    [HarmonyPatch(typeof(AnimalManager), nameof(AnimalManager.sendAnimalAttack))] // specify the target type and target method, also instead of `nameof(AnimalManager.sendAnimalAttack)` you can have "sendAnimalAttack", but if Unturned made a breaking change once you compiled your code you will get an error.
    [HarmonyPrefix] // the patch type.
    static bool PrefixAttack(Animal animal) // arguments from the `sendAnimalAttack` method.
    {
        // Return false to skip Unturned's code, true to allow it.
        return false;
    }
}
```

```cs
static bool PrefixAttack(Animal animal)
{
    if (animal.id == 1)
        return true; // allow damage only for animal with id `1`
    return false; // restrict any other animals to damage.
}
```

```cs
static void PrefixAttack(Animal animal)
{
    // You can also have that method to be `void` it's ok.
}
```

If you will have more patches in your class you might want to have a `[HarmonyPatch(typeof(AnimalManager))]`, so you don't need to specify the type of patching class every time.
```cs
[HarmonyPatch(typeof(AnimalManager))]
class Patch
{
    [HarmonyPatch(nameof(AnimalManager.sendAnimalAttack))]
    [HarmonyPrefix]
    static void PrefixAttack(Animal animal)
    {
    }
    [HarmonyPatch(nameof(AnimalManager.sendAnimalAttack))]
    [HarmonyPostfix]
    static void PostfixAttack(Animal animal)
    {
    }
}
```

To access the `AnimalManager` instance in your patch:

```cs
static bool PrefixAttack(AnimalManager __instance, Animal animal)
{
    // Use __instance for the AnimalManager instance, and yes it has to be named as `__instance` otherwise it won't work.
    return false;
}
```

> **💡 PRO TIP**  
> The best practice is to `try catch` all of your stuff inside of the patch method, so in case if Unturned don't have any try catch inside of the code that might crash the server, and lead to a real troubles, also that just might be useful to understand what's broken in your patch.
> ```cs
> static bool PrefixAttack(AnimalManager __instance, Animal animal)
>{
>    try
>    {
>       if (animal.id == 1)
>           Logger.LogWarning("Found that animal!")
>    }
>    catch (Exception ex)
>    {
>       Logger.LogException(ex, $"An error occured in {nameof(PrefixAttack)}")       
>    }
>    return false;
>}
> ```

#### Postfix Patch Example

To run your code after the original method:

```cs
[HarmonyPatch(typeof(AnimalManager), nameof(AnimalManager.sendAnimalAttack))]
[HarmonyPostfix] // due to postfix (HarmonyPostfix) method is going to be called after `sendAnimalAttack`.
static void PostfixAttack(Animal animal)
{
    // Your code here, runs after the original method.
    // Note: Postfix can't have `bool` return value, it has to be `void`.
}
```

#### Cleanup

You can handle exceptions that are thrown while patching. Use the exception to see what's wrong.

```cs
[HarmonyCleanup]
static Exception Cleanup(Exception ex, MethodBase original)
{
    if (ex != null)
    {
        // Create an event here and subscribe on it in your plugin, and fire that event with exception you got here,
        // and I'd recommend to:
        // 1. Log exception.
        // 2. `UnloadPlugin()`, as your patches are probably broken, either Unturned's made a breaking change or you did something wrong.
    }
    return null;
}
```

# Harmony Documentation

The [Harmony Docs](https://harmony.pardeike.net/articles/intro.html) are your best friend. They explain most things you’ll need. I also recommend searching for examples of Harmony code directly on [GitHub](https://github.com/) (use "harmony" as the search term) or see [OpenMod](https://github.com/openmod/openmod). This often gives better results than using Google or other search engines.

# Conclusion

You’ve learned the basics of Harmony, sure there's a lot of more things to learn. Whether you're a beginner or an experienced developer, I hope you found something useful here. See you next time! -->