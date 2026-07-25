# **Configuração de Ladder**

---

## **Configuração de Ladder**

As **Ladders** (Categorias de Batalha) definem os formatos de matchmaking competitivos e casuais no **Cobblemon BattleHUB**. Elas controlam tudo: tamanho do time, regras de banimento, ajustes de nível e quais mecânicas de batalha (gimmicks) são permitidas na arena.

---

### **Caminho dos Diretórios**

O mod organiza automaticamente as ladders em dois diretórios distintos para manter separadas as configurações padrão e as personalizadas:

- **Diretório Raiz (Ladders Padrão):** `config/cobblemon\_battlehub/ladders/`
- **Diretório Customizado (Criações de Jogadores/Admins):** `config/cobblemon\_battlehub/ladders/custom\_ladders/`

---

## **Ladders Auto-Geradas**

Se a pasta raiz estiver vazia, o servidor irá gerar automaticamente um conjunto completo de formatos padrão:

- **Singles, Doubles e Triples:** Gera versões Casuais (Nv. 50 e Nv. 100) e Ranqueadas (Nv. 50, com bans de OU) para cada tipo.
- **Monotype:** Gera formatos Casuais Nv. 50 e Nv. 100 usando a "Cláusula Mesmo Tipo".
- **Tournaments:** Gera filas específicas para torneios (`tourney_solo`, `tourney_duplas`, `tourney_trios`, `tourney_monotype`).

---

## **Template Padrão de Configuração**

Abaixo está o template JSON padrão para criar ou editar uma Ladder. Ele baseia-se diretamente nos campos da classe `Ladder.java`:

```json
{
  "id": "singles_ranked",
  "queueLabel": "Fila Ranqueada",
  "displayName": "Singles Ranqueados Nv. 50",
  "description": "Formato oficial ranqueado de Singles.",
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
  "allowRestrictedLegendary": false,
  "allowMythical": false,
  "allowParadox": false,
  "allowMega": true,
  "allowZMove": true,
  "allowDynamax": false,
  "allowTera": true,
  "maxSubLegendary": 1,
  "maxRestricted": 1,
  "maxMythical": 1,
  "maxParadox": 1,
  "maxCombinedSpecial": 1
}
```

---

## **Explicação Detalhada dos Parâmetros**

### **1. Identificação e Exibição (UI de Matchmaking)**

- **`id`**: ID interno único da categoria (deve coincidir exatamente com o nome do ficheiro, sem a extensão `.json`).
- **`queueLabel`**: Nome da fila exibido em pacotes de rede e interfaces (ex.: Fila Ranqueada, Fila Rápida, Torneio).
- **`displayName`**: Título amigável mostrado aos jogadores nos menus do mod.
- **`description`**: Breve descrição explicando as regras ou o propósito da fila na interface.
- **`ranked`** (true/false): Determina se a categoria concede pontos de ranking (Elo/Glicko-2) e atualiza o leaderboard global.

### **2. Regras Estruturais de Combate**

- **`battleTypeId`**: Formato do campo de batalha. Valores suportados:
  - "singles" (1v1)
  - "doubles" (2v2)
  - "triples" (3v3)
- **`requiredTeamSize`**: Número mínimo de Pokémon que o jogador deve ter na equipe para entrar na fila (Padrão: 6).
- **`adjustLevel`**: Nível para o qual todos os Pokémon da equipe serão temporariamente ajustados durante a batalha. Use 0 para desativar o ajuste e lutar no nível real do Pokémon.

### **3. Cláusulas Competitivas**

- **`enforceSpeciesClause`** (true/false): Impede que o jogador use dois ou mais Pokémon da mesma espécie na mesma equipe.
- **`enforceItemClause`** (true/false): Impede que dois ou mais Pokémon segurem o mesmo item equipado.

### **4. Filtros de Meta (Restricted, Mythical e Paradox)**

- **`allowRestrictedLegendary`** (true/false): Se desativado, bane pokémon lendários de tier restrito (segundo regras oficiais VGC).
- **`allowMythical`** (true/false): Habilita ou desabilita o uso de pokémon míticos (ex.: Mew, Celebi, Jirachi).
- **`allowParadox`** (true/false): Habilita ou desabilita o uso de pokémon Paradox (ex.: Great Tusk, Iron Valiant).
- **`maxRestricted`**: Quantidade máxima de pokémon lendários restritos permitida na equipe.
- **`maxMythical`**: Quantidade máxima de pokémon míticos permitida na equipe.
- **`maxParadox`**: Quantidade máxima de pokémon paradox permitida na equipe.
- **`maxCombinedSpecial`**: Limite combinado máximo de pokémon de categorias especiais permitidos na equipe.

### **5. Mecânicas da Arena (Gimmicks)**

- **`allowMega`** (true/false): Permite ou bane Mega Evolução na batalha.
- **`allowZMove`** (true/false): Permite ou bane o uso de Z-Moves.
- **`allowDynamax`** (true/false): Permite ou bane as mecânicas Dynamax e Gigantamax.
- **`allowTera`** (true/false): Permite ou bane Terastal (fenômeno de Paldea).

### **6. Listas de Banimento & Regras Customizadas (Showdown)**

- **`banPresets`**: Lista contendo os IDs dos arquivos JSON de presets de ban que você quer herdar (ex.: ["ou"], ["lc"], ["legendaries"]).
- **`bannedSpeciesKeys`**: Lista manual para banir pokémon específicos pelo ID (ex.: ["charizard", "mewtwo"]).
- **`bannedItemKeys`**: Lista manual para banir itens específicos.
- **`bannedAbilityKeys`**: Lista manual para banir habilidades específicas.
- **`bannedMoveKeys`**: Lista manual para banir movimentos específicos.

---

## **Lógica de Sufixos Dinâmicos (Duels Customizados)**

A classe `Ladder.java` inclui um sistema inteligente de **Sufixo Dinâmico** para quando dois jogadores se desafiam diretamente e decidem desativar certas mecânicas na hora (usando a UI de convite do mod).

Se os jogadores escolherem desativar gimmicks no menu, o mod gera uma cópia modificada em memória da Ladder, adicionando os seguintes sufixos invisíveis ao `id`:

- `_nomega` (Desativa Mega Evolution)
- `_noz` (Desativa Z-Moves)
- `_nodyna` (Desativa Dynamax)
- `_notera` (Desativa Terastal)

**Exemplo:** Se sua Ladder base for `singles_ranked` e o jogador desativar Tera na tela de convite, o mod cria temporariamente uma cópia com o ID `singles_ranked_notera` aplicando automaticamente a opção `"allowTera": false`, garantindo que a batalha ocorra de forma justa e sem dessincronização de regras.

---