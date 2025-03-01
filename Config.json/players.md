---
position: 10
title: Players
authors: MCrow
category: servers
published: true
description: Players settings allow you to adjust the health, food, water, and virus levels of players, as well as the damage and regeneration rates of these levels.
---

![players](assets/players.png)

# Players
Players settings allow you to adjust the health, food, water, and virus levels of players, as well as the damage and regeneration rates of these levels. You can also adjust the experience, detection radius, and other player settings.

Based on [Official Unturned Wiki](https://unturned.wiki.gg/wiki/Gameplay_config#Players) and our experience.

```json
"Players": {
  "Health_Default": 100,
  "Health_Regen_Min_Food": 90,
  "Health_Regen_Min_Water": 90,
  "Health_Regen_Ticks": 60,
  "Food_Default": 100,
  "Food_Use_Ticks": 350,
  "Food_Damage_Ticks": 15,
  "Water_Default": 100,
  "Water_Use_Ticks": 320,
  "Water_Damage_Ticks": 20,
  "Virus_Default": 100,
  "Virus_Infect": 50,
  "Virus_Use_Ticks": 125,
  "Virus_Damage_Ticks": 25,
  "Leg_Regen_Ticks": 750,
  "Bleed_Damage_Ticks": 10,
  "Bleed_Regen_Ticks": 750,
  "Armor_Multiplier": 1.0,
  "Experience_Multiplier": 1.5,
  "Detect_Radius_Multiplier": 0.5,
  "Ray_Aggressor_Distance": 8.0,
  "Lose_Skills_PvP": 1.0,
  "Lose_Skills_PvE": 1.0,
  "Lose_Skill_Levels_PvP": 0,
  "Lose_Skill_Levels_PvE": 0,
  "Lose_Experience_PvP": 0.5,
  "Lose_Experience_PvE": 0.5,
  "Lose_Items_PvP": 1.0,
  "Lose_Items_PvE": 1.0,
  "Lose_Clothes_PvP": true,
  "Lose_Clothes_PvE": true,
  "Lose_Weapons_PvP": true,
  "Lose_Weapons_PvE": true,
  "Can_Hurt_Legs": true,
  "Can_Break_Legs": false,
  "Can_Fix_Legs": true,
  "Can_Start_Bleeding": false,
  "Can_Stop_Bleeding": true,
  "Spawn_With_Max_Skills": false,
  "Spawn_With_Stamina_Skills": false,
  "Allow_Instakill_Headshots": false,
  "Allow_Per_Character_Saves": false,
  "Enable_Terrain_Color_Kick": true
}

```

## Health_Default
Determines the amount of health a player spawns with. Must be between **0** and **255**.

## Health_Regen_Min_Food
The minimum amount of food required for health regeneration to start.

## Health_Regen_Min_Water
The minimum amount of water required for health regeneration to start.

## Health_Regen_Ticks
How often health regenerates. A lower value regenerates health faster.

## Food_Default
Determines the amount of food a player spawns with. Must be between **0** and **255**.

## Water_Default
Determines the amount of water a player spawns with. Must be between **0** and **255**.

## Virus_Default
Determines the amount of immunity a player spawns with. Must be between **0** and **255**.

## Virus_Infect
Threshold at which immunity begins to deplete until treated. Setting to **0** disables untreated infections.

## Virus_Use_Ticks
Controls how often immunity is depleted when below the infection threshold. A lower value means faster depletion.

## Virus_Damage_Ticks
How often the player takes damage when immunity is fully depleted.

## Leg_Regen_Ticks
Determines the duration for the "Broken Bones" debuff to expire. A lower value reduces the healing time.

## Bleed_Damage_Ticks
How often the player takes damage from the "Bleeding" debuff.

## Bleed_Regen_Ticks
Determines the duration for the "Bleeding" debuff to expire. A lower value shortens recovery time.

## Armor_Multiplier
Determines the multiplier on any damage the player receives. Values below **1.0** reduce damage and values above **1.0** increase damage.

## Experience_Multiplier
Determines the multiplier on the amount of experience gained by players.

## Detect_Radius_Multiplier
Determines the multiplier on how easily animals and zombies detect players. Higher values make detection easier.

## Ray_Aggressor_Distance
Determines the range in meters used to determine the "aggressor" during combat. Relevant for only the reputation systems.

## Lose_Skills_PvP
Determines the percentage of skills retained after a PvP related death. If set to **1** all of the player's skills will be kept on death.

## Lose_Skills_PvE
Determines the percentage of skills retained after a PvE related death. If set to **1** all of the player's skills will be kept on death.

## Lose_Skill_Levels_PvP
Determines the number of skill levels lost after a PvP related death. If set to **0** the player will not lose any skill levels on death.

## Lose_Skill_Levels_PvE
Determines the number of skill levels l lost after a PvE related death.. If set to **0** the player will not lose any skill levels on death.

## Lose_Experience_PvP
Determines the percentage of experience retained after a PvP related death. If set to **1** the player will not lose any experience on death.

## Lose_Experience_PvE
Determines the percentage of experience retained after a PvE related death. If set to **1** the player will not lose any experience on death.

## Lose_Items_PvP
Determines the percentage of items lost upon PvP-related death. If set to **0** all items will be lost on death. Keep in mind skillset loadouts override this setting.

## Lose_Items_PvE
Determines the percentage of items lost upon PvE-related death. If set to **0** all items will be lost on death. Keep in mind skillset loadouts override this setting.

## Lose_Clothes_PvP
Determines if clothing items are dropped on PvP death.

## Lose_Clothes_PvE
Determines if clothing items are dropped on PvE death.

## Lose_Weapons_PvP
Determines if weapons in the Primary and Secondary slots are dropped on PvP related deaths.

## Lose_Weapons_PvE
Determines if weapons in the Primary and Secondary slots are dropped on PvE related deaths.

## Can_Hurt_Legs
Determines if players can take fall damage.

## Can_Break_Legs
Determines whether players can gain the "Broken Bones" debuff.

## Can_Fix_Legs
Determines whether players can naturally recover from the "Broken Bones" debuff over time.

## Can_Start_Bleeding
Determines whether players can receive the "Bleeding" debuff.

## Can_Stop_Bleeding
Determines whether players can naturally recover from the "Bleeding" debuff over time.

## Spawn_With_Max_Skills
Determines whether players spawn with all skills at their maximum level.

## Spawn_With_Stamina_Skills
Determines whether players spawn with Cardio, Exercise, Diving, and Parkour skills at maximum level.

## Allow_Instakill_Headshots
Determines whether players can be instantly killed by a headshot using ranged weapons like Timberwolf, Grizzly, and Ekho.

## Allow_Per_Character_Saves
Allows players to use multiple character saves, enabling different inventories and stats per character. This is enabled by default in single player and disabled in multiplayer.


