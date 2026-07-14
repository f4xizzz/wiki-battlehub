# **Products and Bundles**

---

## **Products and Bundles**

The **Cobblemon BattleHUB** shop is divided into two parts: the static storefront (fixed products and bundles) and the rotation roulette. The `shop.json` file manages the static section, where you can sell items, Pokémon bundles, cosmetics (via commands), or perks (VIPs).

---

### **File Path**

`config/cobblemon_battlehub/shop.json`

---

## **JSON Structure**

Below is an example of the structure generated automatically by the mod, containing a `"Bundle"` (a package with multiple commands/Pokémon) and a regular `"Item"`:

        {  
          "shop": [  
            {  
              "id": "example_bundle",  
              "name": "Tinkaton Bundle",  
              "type": "bundle",  
              "description": "Purchase this amazing pink bundle with the full evolution line!",  
              "price": 1500.0,  
              "discount": 15,  
              "currency": "Dollars",  
              "currencyName": "<green><bold>%price%$</bold></green>",  
              "featured": true,  
              "scale": 1.3,  
              "offsetX": 0,  
              "offsetY": 0,  
              "width": 120,  
              "height": 160,  
              "maxPurchases": 1,  
              "imageUrls": [  
                "https://i.imgur.com/ZGNJEw7.png"  
              ],  
              "includedItems": [  
                "Shiny Tinkaton with Perfect IVs",  
                "5x Diamonds"  
              ],  
              "commands": [  
                "pokegiveother %player% tinkaton s min_perfect_ivs=6",  
                "give %player% diamond 5"  
              ]  
            },  
            {  
              "id": "abilitypatch",  
              "name": "AbilityPatch",  
              "type": "item",  
              "description": "Changes your Pokémon's ability to its Hidden Ability.",  
              "price": 1000.0,  
              "discount": 0,  
              "currency": "Dollars",  
              "currencyName": "<green><bold>%price%$</bold></green>",  
              "featured": false,  
              "scale": 1.0,  
              "offsetX": 0,  
              "offsetY": 0,  
              "width": 60,  
              "height": 60,  
              "maxPurchases": 0,  
              "imageUrls": [  
                "https://i.imgur.com/T9VPbyx.png"  
              ],  
              "includedItems": [  
                "1x Ability Patch"  
              ],  
              "commands": [  
                "give %player% cobblemon:ability_patch"  
              ]  
            }  
          ]  
        }

---

## **Parameter Explanation**

### **1. Identification and Metadata**

* **`id`**: Unique string that identifies the product in the database. 
!!! warning "**Warning:**"
    Never change a product’s ID after players have purchased it, or they will lose purchase history and may bypass purchase limits.  
* **`name`**: The product name (supports MiniMessage and legacy colors).  
* **`type`**: Defines how the product is displayed. Common values are `"item"` or `"bundle"`.  
* **`description`**: The description text shown in the item tooltip.  
* **`featured`**: (true/false) If true, the product appears highlighted in the top shelf of the store at a larger size.

### **2. Economy and Pricing**

* **`price`**: The product base price.  
* **`discount`**: Discount percentage value (e.g. `15` = 15% off). The mod displays a promotional badge visually.  
* **`currency`**: The internal currency ID that ImpactorBridge will charge the player.  
* **`currencyName`**: The visual formatting of how currency and price appear in the menu. Use `%price%` where the number should be inserted.

### **3. Visual Rendering (GUI)**

* **`imageUrls`**: List of direct image links (e.g. Imgur). The mod downloads, caches them via WebImageCache, and renders them in the menu. If there is more than one, they act like an animated GIF.  
* **`width / height`**: Base resolution at which the image is drawn in the menu.  
* **`scale`**: Icon size multiplier in the menu (default: `1.0`).  
* **`offsetX / offsetY`**: Fine pixel adjustments to move the image if it appears off-center on the card.

### **4. Limits and Rewards**

* **`maxPurchases`**: Maximum number of times a player can buy this item in their lifetime. If set to `0`, the item can be purchased infinitely.  
* **`includedItems`**: A list of texts shown in the floating menu tooltip describing what is included in the bundle.  
* **`commands`**: The list of console commands executed when the purchase is approved. The `%player%` variable is replaced with the buyer’s username.

---