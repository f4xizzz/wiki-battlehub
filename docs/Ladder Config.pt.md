# **Ladder Config**

---

# **Configuração de Ladders**

As **Ladders** (Categorias de Batalha) definem os formatos competitivos e casuais de matchmaking no **Cobblemon BattleHUB**. Elas controlam tudo: o tamanho da equipe, as regras de banimento, os ajustes de nível e quais mecânicas (gimmicks) de batalha são permitidas na arena.

---

### **Caminho do Diretório**

`config/cobblemon_battlehub/ladders/`

---

## **Modelo Padrão de Configuração**

Abaixo está o modelo JSON padrão para criar ou editar uma Ladder. Ele é baseado diretamente nos campos da classe Ladder.java:

        {  
          "id": "singles_ranked",  
          "queueLabel": "Ranked Queue",  
          "displayName": "Ranked Singles Lv. 50",  
          "description": "Official Ranked Singles format.",  
          "ranked": true,  
          "battleTypeId": "singles",  
          "requiredTeamSize": 6,  
          "adjustLevel": 50,  
          "enforceSpeciesClause": true,  
          "enforceItemClause": true,  
          "banPresets": [  
            "ou"  
          ],  
          "bannedSpeciesKeys": [],  
          "bannedItemKeys": [],  
          "bannedAbilityKeys": [],  
          "bannedMoveKeys": [],  
          "bannedTierKeys": [],  
          "showdownRules": [],  
          "allowRestrictedPokemon": false,  
          "allowMythical": false,  
          "allowParadox": false,  
          "allowMega": true,  
          "allowZMove": true,  
          "allowDynamax": false,  
          "allowTera": true  
        }

---

## **Explicação Detalhada dos Parâmetros**

### **1\. Identificação e Display (Visual do Matchmaking)**

* **`id`**: ID único interno da categoria (deve bater exatamente com o nome do arquivo .json sem a extensão).  
* **`queueLabel`**: Nome da fila que aparece nos pacotes de rede e interfaces (ex: Ranked Queue, Quick Queue, Tournament).  
* **`displayName`**: Título amigável exibido aos jogadores dentro dos menus do mod.  
* **`description`**: Breve descrição que explica as regras ou o intuito da fila na interface.  
* **`ranked`** (true/false): Define se a categoria vale pontos de classificação (Elo/Glicko-2) e atualiza o ranking global.

### **2\. Regras Estruturais de Combate**

* **`battleTypeId`**: O formato de campo do combate. Valores suportados:  
  * "singles" (1v1)  
  * "doubles" (2v2)  
  * "triples" (3v3)  
* **`requiredTeamSize`**: Quantidade mínima de Pokémon que o jogador precisa ter na party para poder entrar na fila de matchmaking (Padrão: 6).  
* **`adjustLevel`**: Nível para o qual todos os Pokémon da equipa serão ajustados temporariamente durante a batalha. Use 0 para desativar o ajuste e lutar com os níveis reais do jogo.

### **3\. Cláusulas Competitivas**

* **`enforceSpeciesClause`** (true/false): Proíbe que o jogador utilize dois ou mais Pokémon da mesma espécie na mesma equipa.  
* **`enforceItemClause`** (true/false): Proíbe que dois ou mais Pokémon segurem o mesmo item equipado.

### **4\. Filtros de Meta (Míticos, Lendários e Paradoxos)**

* **`allowRestrictedPokemon`** (true/false): Se desativado, proíbe Pokémon lendários de tier restrita (conforme as regras oficiais da VGC).  
* **`allowMythical`** (true/false): Ativa ou desativa a utilização de Pokémon Míticos (ex: Mew, Celebi, Jirachi).  
* **`allowParadox`** (true/false): Ativa ou desativa a utilização de Pokémon Paradoxos (ex: Great Tusk, Iron Valiant).

### **5\. Mecânicas de Arena (Gimmicks)**

* **`allowMega`** (true/false): Permite ou bane a Mega Evolução na batalha.  
* **`allowZMove`** (true/false): Permite ou bane o uso de Z-Moves.  
* **`allowDynamax`** (true/false): Permite ou bane as mecânicas de Dynamax e Gigantamax.  
* **`allowTera`** (true/false): Permite ou bane o Terastal (Fenómeno de Paldea).

### **6\. Listas de Banimento & Regras Customizadas (Showdown)**

* **`banPresets`**: Lista contendo os IDs dos ficheiros JSON de banimento que você quer herdar (ex: \["ou"\], \["lc"\], \["legendaries"\]).  
* **`bannedSpeciesKeys`**: Lista manual para banir Pokémon específicos por ID (ex: \["charizard", "mewtwo"\]).  
* **`bannedItemKeys`**: Lista manual para banir itens específicos.  
* **`bannedAbilityKeys`**: Lista manual para banir Habilidades específicas.  
* **`bannedMoveKeys`**: Lista manual para banir Ataques específicos.  
* **`showdownRules`**: Permite injetar regras adicionais interpretadas diretamente pela Showdown no motor do Cobblemon (ex: \["Same Type Clause"\] para Monotype).

---

## **A Lógica dos Sufixos Dinâmicos (Duelos Personalizados)**

A classe Ladder.java possui um sistema inteligente de **Sufixos Dinâmicos** para quando dois jogadores se convidam para um duelo direto e decidem desativar certas mecânicas na hora (usando a interface visual de convite do mod).

Se os jogadores escolherem desativar gimmicks no menu, o mod gera em memória uma cópia modificada da sua Ladder adicionando os seguintes sufixos invisíveis ao ID:

* `_nomega` (Desativa Mega Evolução)  
* `_noz` (Desativa Z-Moves)  
* `_nodyna` (Desativa Dynamax)  
* `_notera` (Desativa Terastal)

**Exemplo:** Se a sua Ladder base for `singles_ranked` e o jogador desativar o Tera na tela de convite, o mod cria temporariamente uma cópia sob o ID `singles_ranked_notera` com a opção `"allowTera": false` aplicada automaticamente, garantindo que o combate ocorra de forma justa e sem dessincronização de regras.

---