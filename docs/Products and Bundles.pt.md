# **Products and Bundles**

---

## **Produtos e Pacotes**

A loja do **Cobblemon BattleHUB** é dividida em duas partes: a vitrine estática (produtos fixos e pacotes) e a roleta de rotação. O arquivo shop.json é responsável por gerenciar a parte estática, onde você pode vender itens, bundles de Pokémon, cosméticos (via comandos) ou benefícios (VIPs).

---

### **Caminho do Arquivo**

`config/cobblemon_battlehub/shop.json`

---

## **Estrutura do JSON**

Abaixo está um exemplo da estrutura gerada automaticamente pelo mod, contendo um `"Bundle"` (pacote com múltiplos comandos/Pokémon) e um `"Item"` comum:

        {  
          "shop": [  
            {  
              "id": "example_bundle",  
              "name": "Tinkaton Bundle",  
              "type": "bundle",  
              "description": "Adquira este incrível pacote rosa com a linha evolutiva completa!",  
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
                "Shiny Tinkaton com IVs Perfeitos",  
                "5x Diamantes"  
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
              "description": "Altera a habilidade do seu Pokémon para a Hidden Ability.",  
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

## **Explicação dos Parâmetros**

### **1\. Identificação e Metadados**

* **`id`**: String única que identifica o produto no banco de dados. 
!!! warning "**Atenção:**"
    Nunca altere o ID de um produto após jogadores o terem comprado, senão eles perderão o histórico de compras e poderão comprar novamente burlar limites.  
* **`name`**: O nome do produto (Suporta MiniMessage e cores legadas).  
* **`type`**: Define visualmente o produto. Geralmente usa-se "item" ou "bundle".  
* **`description`**: O texto de descrição exibido na dica de ferramenta (tooltip) do item.  
* **`featured`**: (true/false) Se for verdadeiro, o produto aparecerá em destaque na prateleira superior da loja em tamanho maior.

### **2\. Economia e Valores**

* **`price`**: Valor base do produto.  
* **`discount`**: Valor em porcentagem de desconto (Ex: 15 \= 15% de desconto). O mod exibirá uma etiqueta de promoção visual.  
* **`currency`**: O ID interno da moeda que o sistema ImpactorBridge irá cobrar do jogador.  
* **`currencyName`**: A formatação visual de como a moeda e o preço aparecem no menu. Use %price% onde o número deve ser inserido.

### **3\. Renderização Visual (GUI)**

* **`imageUrls`**: Lista de links diretos para imagens (ex: Imgur). O mod faz o download, armazena em cache (via WebImageCache) e as renderiza no menu. Se tiver mais de uma, elas atuarão como um GIF animado.  
* **`width / height`**: Resolução base na qual a imagem será desenhada no menu.  
* **`scale`**: Multiplicador de tamanho do ícone no menu (Padrão: 1.0).  
* **`offsetX / offsetY`**: Ajustes finos em pixels para mover a imagem caso ela fique descentralizada no card.

### **4\. Limites e Recompensas**

* **`maxPurchases`**: Limite máximo de vezes que um jogador pode comprar este item na vida. Se definido como 0, o item pode ser comprado infinitas vezes.  
* **`includedItems`**: Uma lista de textos exibida no menu flutuante (tooltip) mostrando o que vem no pacote.  
* **`commands`**: A lista de comandos executada pelo console quando a compra é aprovada. A variável %player% é substituída pelo nick de quem comprou.

---