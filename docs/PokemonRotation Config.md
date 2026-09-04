# **PokemonRotation Config**

---

## **Pokémon Rotation Configuration**

The Pokémon rotation is the shop's rotating stock. Every `intervalHours`, the mod rolls a fresh list of Pokémon: it picks a **pool** (weighted), rolls an **IV tier** (weighted), rolls Shiny, computes a price from those, and lists that many cards in the shop's rotation tab. Each card can be bought once.

### **File Path**

`config/cobblemon_battlehub/pkmrotation_config.json`

---

## **Default Configuration Template**

The real generated file has **8 pools** (Common, Uncommon, Rare, Ultra-Rare, Legendary, Mythical, Ultra Beast, Paradox) and **6 IV tiers** (1 through 6 perfect IVs). Trimmed here for readability:

```json
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
    { "perfectIvs": 1, "weight": 50, "priceMultiplier": 1.0 },
    { "perfectIvs": 6, "weight": 1,  "priceMultiplier": 6.0 }
  ]
}
```

---

## **Detailed Parameter Explanation**

### **1. Global Rotation Parameters**

* **`intervalHours`** (Default: 12): Hours between automatic rotations. The next rotation time is stamped when a rotation is generated.
* **`pokemonsPerRotation`** (Default: 12): How many Pokémon cards the rotation generates each cycle.
* **`shinyChancePercent`** (Default: 2.5): Percentage chance (0–100) that each rolled Pokémon is Shiny.
* **`shinyPriceMultiplier`** (Default: 3.0): Extra multiplier on the final price when the roll came out Shiny.
* **`currency`** (Default: `"Dollars"`): The economy currency ID charged on purchase, via the Impactor bridge. See [Products and Bundles](Products and Bundles.md) and [Storage](Storage.md).
* **`currencyName`** (Default: `<green><bold>%price%$</bold></green>`): How the price is drawn on the card. MiniMessage formatting is supported; `%price%` is replaced with the computed number.

### **2. Pools (`pools`)**

Each pool is one rarity bucket:

* **`name`**: A label only — it is shown (uppercased) in the card's description text. It does **not** filter Pokémon; the `pokemons` list is the real content.
* **`weight`**: Relative odds of this pool being picked for a card, versus the sum of all pool weights. With Common `80` and Legendary `3`, Common is ~27× more likely per card.
* **`basePrice`**: The starting price for a Pokémon from this pool, before IV and Shiny multipliers.
* **`pokemons`**: Lowercase Cobblemon species IDs eligible from this pool. Add or remove freely.

### **3. IV Tiers (`ivTiers`)**

Each roll also draws an IV tier (independent weighted roll):

* **`perfectIvs`**: Number of guaranteed 31-IV stats the sold Pokémon gets (passed to the give command as `min_perfect_ivs`).
* **`weight`**: Relative odds of this tier, versus the sum of all tier weights.
* **`priceMultiplier`**: Multiplies the pool's `basePrice`.

### **How the price is calculated**

```
finalPrice = pool.basePrice
           × ivTier.priceMultiplier
           × (shinyPriceMultiplier if the roll is Shiny)
```

Example: a `Rare` Pokémon (`basePrice 3000`) at the 4-perfect-IV tier (`priceMultiplier 2.0`), Shiny (`3.0`) → `3000 × 2.0 × 3.0 = 18000`.

---

## **Reload**

`/bh reload` reloads the config. To discard the current stock and force a brand-new rotation immediately, use `/bh rollpokemonshop`.
