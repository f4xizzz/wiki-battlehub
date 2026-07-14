# **Ban Presets**

---

## **Ban Presets**

To prevent server administrators from manually typing long lists of banned Pokémon for each battle category (Ladder), **Cobblemon BattleHUB** introduces the **Ban Presets** system.

These presets are simple JSON files that store predefined Pokémon lists. Any Ladder can load one or more of these presets at once by referencing the filename.

### **Directory Path**

`config/cobblemon_battlehub/ladders/ban_presets/`

## **How the Cascade Logic Works (Smogon Tiers)**

The mod automatically generates lists based on Smogon's official competitive divisions. To simplify maintenance and avoid huge repetitive files, the mod uses a **cascade logic**.

That means lower tiers automatically inherit all bans from the tiers above them. Here is the direct inheritance flow:

        [Ubers (Base bans)]  
             ↓  
        [OU (Ubers + OU bans)]  
             ↓  
        [UU (OU + UU bans)]  
             ↓  
        [RU (UU + RU bans)]  
             ↓  
        [NU (RU + NU bans)]  
             ↓  
        [PU (NU + PU bans)]

* If you use the `ou` preset, it will ban Pokémon banned in OU **plus** all Ubers.  
* If you use the `pu` preset, it will ban Pokémon banned in PU **plus** all of NU, RU, UU, OU, and Ubers at once.  
* **Note:** Specific formats like `lc` (Little Cup), `doubles_ou`, and `monotype` have their own inheritance or separate ban lists adapted to their particular rules.

---

## **Automatically Generated Base Presets**

On the first mod startup, the following JSON files are automatically generated in the presets folder:

### **Special Category Lists**

| Preset Name | Description |
| :---- | :---- |
| `legendaries` | Contains all Legendary and Mythical Pokémon (e.g. Mewtwo, Lugia, Zacian, Koraidon). |
| `ultra_beasts` | Contains all Ultra Beasts in the Pokémon ecosystem (e.g. Nihilego, Buzzwole, Kartana). |
| `paradoxes` | Contains all Paradox Pokémon from the past and future (e.g. Great Tusk, Iron Valiant). |

---

### **Official Smogon Tier Lists**

| Preset Name | Smogon Format / Reference |
| :---- | :---- |
| `ubers` | Pokémon banned from the Ubers category (Anything Goes, e.g. Calyrex-Shadow). |
| `ou` | Pokémon banned from the main Overused (OU) tier + Ubers. |
| `uu` | Pokémon banned from Underused (UU) + OU + Ubers. |
| `ru` | Pokémon banned from Rarelyused (RU) + UU + OU + Ubers. |
| `nu` | Pokémon banned from Neverused (NU) + RU + UU + OU + Ubers. |
| `pu` | Pokémon banned from PU + NU + RU + UU + OU + Ubers. |
| `lc` | Pokémon banned from Little Cup format (level 5 Pokémon that are too strong). |
| `doubles_ou` | Pokémon banned in Doubles OU format. |
| `monotype` | Pokémon banned specifically in Monotype format. |

---

## **How to Create a Custom Ban Preset**

The BattleHUB system is designed to load **any** `.json` file placed inside the `ban_presets` folder. You can create unique rules and formats for your server events in seconds.

### **Step 1: Create the JSON file**

Go to the `ban_presets/` folder and create a file with the name you want. The filename (without `.json`) will be the preset ID.

* *Example:* `no_starters_event.json`

### **Step 2: Add the Pokémon List**

Add the desired Pokémon as a JSON array. The system is smart and automatically converts names to lowercase, and ignores the `cobblemon:` prefix if added by mistake:

        [  
          "charizard",  
          "blastoise",  
          "venusaur",  
          "cobblemon:meowscarada"  
        ]

### **Step 3: Use the Preset in a Ladder**

Open the configuration of your desired Ladder (in the `ladders/` folder) and add your preset ID to the `"banPresets"` list:

        {  
          "id": "event_singles",  
          "displayName": "Event Battle",  
          "banPresets": [  
            "no_starters_event"  
          ]  
        }

### **Step 4: Reload**

To apply and sync the new files, run the reload command from the console or as an in-game administrator:

`/bh reload`

---