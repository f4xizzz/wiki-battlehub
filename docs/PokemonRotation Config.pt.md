# **PokemonRotation Config**

---

## **Configuração da Rotação de Pokémon**

O sistema de rotação de Pokémon do **Cobblemon BattleHUB** funciona como uma "vitrine dinâmica" ou "roleta". De tempos em tempos (configuráveis), o mod sorteia uma lista de Pokémon com status (IVs) e chances de brilhantes (Shiny) aleatórios para serem expostos na loja com preços calculados em tempo real.

### **Caminho do Arquivo**

`config/cobblemon_battlehub/pkmrotation_config.json`

---

## **Modelo Padrão de Configuração**

Abaixo está o modelo JSON padrão gerado automaticamente na primeira inicialização do mod:

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

## **Explicação Detalhada dos Parâmetros**

---


### **1\. Parâmetros Globais da Rotação**

* **`intervalHours`** (Padrão: 12): O intervalo de tempo (em horas) para a roleta girar automaticamente e gerar uma nova lista de Pokémon à venda.  
* **`pokemonsPerRotation`** (Padrão: 12): A quantidade de cards de Pokémon que serão expostos simultaneamente na aba de rotação da loja.  
* **`shinyChancePercent`** (Padrão: 2.5): A probabilidade (em porcentagem, de 0.0 a 100.0) de cada Pokémon sorteado nascer Shiny (Brilhante).  
* **`shinyPriceMultiplier`** (Padrão: 3.0): O multiplicador aplicado ao preço final caso o Pokémon sorteado venha Shiny.  
* **`currency`**: O ID interno da moeda do ImpactorBridge que será cobrada (Ex: "Dollars" ou "BattlePoints").  
* **`currencyName`**: A formatação visual do preço no menu. Suporta cores do MiniMessage e a variável %price%.

---
