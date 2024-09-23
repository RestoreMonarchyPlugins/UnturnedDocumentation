---
position: 7
title: Animals
authors: MCrow
published: true
---

![](assets/animals.png)

Animals settings can control the number of animals that spawn on your server, their armor and damage multipliers.

```json
"Animals": {
  "Respawn_Time": 180.0,
  "Damage_Multiplier": 0.75,
  "Armor_Multiplier": 1.25,
  "Max_Instances_Tiny": 4,
  "Max_Instances_Small": 8,
  "Max_Instances_Medium": 16,
  "Max_Instances_Large": 32,
  "Max_Instances_Insane": 64,
  "Weapons_Use_Player_Damage": false
}
```

### Respawn_Time
How many seconds it takes for an animal spawn node to attempt to spawn a new animal.  

I recommend leaving this value at **180**.

### Damage_Multiplier
Multiplier on the damage dealt by animals. When greater than **1**, more damage is dealt.  

If you set `Damage_Multiplier` to 2, animals will deal twice as much damage as normal. 
- **Bears** deal **20** damage per hit, so with a multiplier of **2**, they will deal **40** damage per hit.
- **Wolves** deal **10** damage per hit, so with a multiplier of **2**, they will deal **20** damage per hit.

> **💡 PRO TIP**
> Only bears and wolves attack players.

### Armor_Multiplier
Multiplier on the damage dealt to animals. When greater than **1**, more damage is dealt. 

If you want to make animals harder to kill, you can set this value to **0.5.** For example:
- **Cow** has **100** HP, so with `Armor_Multiplier` set to **0.5** player will have to deal **200** HP damage to kill it.
- **Pig** has **50** HP, so with `Armor_Multiplier` set to **0.5** player will have to deal **100** HP damage to kill it.

### Max_Instances
The maximum number of animals that can exist at the same time on a map of specific size.

- **Tiny** - no official maps
- **Small** - Alpha Valley, Monolith
- **Medium** - PEI, Washington, Yukon
- **Large** - Russia, Germany
- **Insane** - no official maps

For example, if you're server is using **Washington** map and `Max_Instances_Medium` is set to **16** then the maximum number of animals spawning and walking around the map will is just **16**.

I recommend increasing this value to make animals more common on your server. However remember that the number of animals that will spawn naturally is also limited by the number of spawn-points on every map. Washington only has **59** spawn-points for animals, so even if you set `Max_Instances_Medium` to **64**, only **59** animals will spawn.

Number of spawn-points on official survival maps:
- **PEI** - 60
- **Washington** - 59
- **Yukon** - 35
- **Russia** - 85
- **Germany** - 126

### Weapons_Use_Player_Damage
Whether or not a weapon's player damage values should be used instead of the weapon's animals damage values. Most weapons do less damage to players than they do to zombies. When set to **true**, weapons will usually be weaker against animals than they are normally. 