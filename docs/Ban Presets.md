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

```text
ubers   (base list)
  └─ ou    = ubers + OU bans
       └─ uu    = ou + UU bans
            └─ ru    = uu + RU bans
                 └─ nu    = ru + NU bans
                      └─ pu    = nu + PU bans
```

* If you use the `ou` preset, it bans everything banned in OU **plus** all of Ubers.
* If you use the `pu` preset, it bans PU's list **plus** all of NU, RU, UU, OU, and Ubers at once.
* `monotype` builds on top of `ou` (OU bans + a few Pokémon that are broken specifically in Monotype).
* `doubles_ou`, `lc`, `legendaries`, `ultra_beasts`, and `paradoxes` are **standalone** lists — they don't inherit from anything.

---

## **Automatically Generated Base Presets**

On the first mod startup, the following JSON files are automatically generated in the presets folder:

### **Special Category Lists**

| Preset Name | Description |
| :---- | :---- |
| `legendaries` | Legendary and sub-legendary Pokémon (Mewtwo, Lugia, Zacian, Koraidon, the genies, the Treasures of Ruin…). It does **not** include Mythicals like Mew or Celebi — ban those by hand if you need to. |
| `ultra_beasts` | The 11 Ultra Beasts (Nihilego, Buzzwole, Kartana, Naganadel…). |
| `paradoxes` | All 20 Paradox Pokémon, past and future (Great Tusk, Iron Valiant, Roaring Moon, Walking Wake…). |

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

Add the Pokémon as a plain JSON array of species IDs. Names are lowercased automatically and a stray `cobblemon:` prefix is stripped. Use the **form-merged** style with no separators for regional/special forms, matching the bundled lists (`vulpixalola`, `calyrexshadow`, `sneaselhisui`):

```json
[
  "charizard",
  "blastoise",
  "venusaur",
  "cobblemon:meowscarada"
]
```

### **Step 3: Use the Preset in a Ladder**

Open your Ladder file (in the `ladders/` folder) and add the preset ID to `"banPresets"` — you can list several, and they stack with each other and with the ladder's own `bannedSpeciesKeys`:

```json
{
  "id": "event_singles",
  "displayName": "Event Battle",
  "banPresets": ["no_starters_event", "legendaries"]
}
```

### **Step 4: Reload**

To apply and sync the new files, run the reload command from the console or as an in-game administrator:

`/bh reload`

---