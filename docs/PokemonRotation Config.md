# **PokemonRotation Config**

---

## **Pokémon Rotation Configuration**

The Pokémon rotation system in **Cobblemon BattleHUB** works like a dynamic showcase or a roulette. At configurable intervals, the mod selects a list of Pokémon with randomized stats (IVs) and Shiny odds to display in the shop with prices calculated in real time.

### **File Path**

`config/cobblemon_battlehub/pkmrotation_config.json`

---

## **Default Configuration Template**

Below is the default JSON template generated automatically on the first mod startup:

        {  
          "intervalHours": 12,  
          "pokemonsPerRotation": 12,  
          "shinyChancePercent": 2.5,  
          "shinyPriceMultiplier": 3.0,  
          "currency": "Dollars",  
          "currencyName": "<green><bold>%price%$</bold></green>",  
          "pools": [  
            {  
              "name": "Common",  
              "weight": 80,  
              "basePrice": 500.0,  
              "pokemons": ["absol", "bidoof", "gastly", "geodude", "psyduck"]  
            },  
            {  
              "name": "Legendary",  
              "weight": 3,  
              "basePrice": 15000.0,  
              "pokemons": ["articuno", "mewtwo", "rayquaza", "lugia"]  
            }  
          ],  
          "ivTiers": [  
            {  
              "perfectIvs": 1,  
              "weight": 50,  
              "priceMultiplier": 1.0  
            },  
            {  
              "perfectIvs": 6,  
              "weight": 1,  
              "priceMultiplier": 6.0  
            }  
          ]  
        }

## **Detailed Parameter Explanation**

---

### **1. Global Rotation Parameters**

* **`intervalHours`** (Default: 12): The time interval (in hours) for the rotation to automatically refresh and generate a new list of Pokémon for sale.  
* **`pokemonsPerRotation`** (Default: 12): The number of Pokémon cards displayed simultaneously in the shop rotation tab.  
* **`shinyChancePercent`** (Default: 2.5): The chance (percentage, from 0.0 to 100.0) for each selected Pokémon to be Shiny.  
* **`shinyPriceMultiplier`** (Default: 3.0): The multiplier applied to the final price if the selected Pokémon is Shiny.  
* **`currency`**: The internal currency ID used by ImpactorBridge for purchases (e.g. "Dollars" or "BattlePoints").  
* **`currencyName`**: The visual price format in the menu. Supports MiniMessage colors and the %price% variable.

---
