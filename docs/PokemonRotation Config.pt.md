# **PokemonRotation Config**

---

## **Configuração da Rotação de Pokémon**

A rotação de Pokémon é o estoque rotativo da loja. A cada `intervalHours`, o mod sorteia uma lista nova de Pokémon: escolhe um **pool** (por peso), sorteia um **tier de IV** (por peso), sorteia Shiny, calcula um preço a partir disso e lista essa quantidade de cards na aba de rotação da loja. Cada card pode ser comprado uma vez.

### **Caminho do Arquivo**

`config/cobblemon_battlehub/pkmrotation_config.json`

---

## **Modelo Padrão de Configuração**

O arquivo gerado de verdade tem **8 pools** (Common, Uncommon, Rare, Ultra-Rare, Legendary, Mythical, Ultra Beast, Paradox) e **6 tiers de IV** (de 1 a 6 IVs perfeitos). Encurtado aqui para leitura:

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

## **Explicação Detalhada dos Parâmetros**

### **1. Parâmetros Globais da Rotação**

* **`intervalHours`** (Padrão: 12): Horas entre rotações automáticas. O horário da próxima rotação é gravado quando uma rotação é gerada.
* **`pokemonsPerRotation`** (Padrão: 12): Quantos cards de Pokémon a rotação gera por ciclo.
* **`shinyChancePercent`** (Padrão: 2.5): Chance em porcentagem (0–100) de cada Pokémon sorteado sair Shiny.
* **`shinyPriceMultiplier`** (Padrão: 3.0): Multiplicador extra no preço final quando o sorteio saiu Shiny.
* **`currency`** (Padrão: `"Dollars"`): O ID da moeda da economia cobrada na compra, via ponte do Impactor. Veja [Products and Bundles](Products and Bundles.md) e [Storage](Storage.md).
* **`currencyName`** (Padrão: `<green><bold>%price%$</bold></green>`): Como o preço é desenhado no card. Formatação MiniMessage é suportada; `%price%` é trocado pelo número calculado.

### **2. Pools (`pools`)**

Cada pool é um balde de raridade:

* **`name`**: Só um rótulo — aparece (em maiúsculas) no texto de descrição do card. **Não** filtra Pokémon; a lista `pokemons` é o conteúdo de verdade.
* **`weight`**: Chance relativa desse pool ser escolhido para um card, contra a soma dos pesos de todos os pools. Com Common `80` e Legendary `3`, Common é ~27× mais provável por card.
* **`basePrice`**: O preço inicial de um Pokémon desse pool, antes dos multiplicadores de IV e Shiny.
* **`pokemons`**: IDs de espécie do Cobblemon (minúsculo) elegíveis nesse pool. Adicione ou remova à vontade.

### **3. Tiers de IV (`ivTiers`)**

Cada sorteio também tira um tier de IV (sorteio por peso independente):

* **`perfectIvs`**: Quantos status com IV 31 garantidos o Pokémon vendido recebe (passado ao comando de give como `min_perfect_ivs`).
* **`weight`**: Chance relativa desse tier, contra a soma dos pesos de todos os tiers.
* **`priceMultiplier`**: Multiplica o `basePrice` do pool.

### **Como o preço é calculado**

```
precoFinal = pool.basePrice
           × ivTier.priceMultiplier
           × (shinyPriceMultiplier se o sorteio saiu Shiny)
```

Exemplo: um Pokémon `Rare` (`basePrice 3000`) no tier de 4 IVs perfeitos (`priceMultiplier 2.0`), Shiny (`3.0`) → `3000 × 2.0 × 3.0 = 18000`.

---

## **Reload**

`/bh reload` recarrega a config. Para descartar o estoque atual e forçar uma rotação nova na hora, use `/bh rollpokemonshop`.
