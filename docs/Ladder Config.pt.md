# **Configuração de Ladder**

---

## **Configuração de Ladder**

As **Ladders** (Categorias de Batalha) definem os formatos de matchmaking competitivos e casuais no **Cobblemon BattleHUB**. Elas controlam tudo: tamanho do time, regras de banimento, ajustes de nível e quais mecânicas de batalha (gimmicks) são permitidas na arena.

Cada ladder é um arquivo `.json`. Quais delas realmente aparecem na fila é decidido pelos campos `activeRankedLadders` / `activeCasualLadders` do [`server_config.json`](Server Config.md) — um arquivo de ladder que não está listado lá ainda carrega, mas nunca fica disponível na fila.

---

### **Caminho dos Diretórios**

O mod organiza as ladders em dois diretórios para manter separadas as padrão e as personalizadas:

- **Diretório Raiz (Ladders Padrão):** `config/cobblemon_battlehub/ladders/`
- **Diretório Customizado (Criações de Jogadores/Admins):** `config/cobblemon_battlehub/ladders/custom_ladders/`

---

## **Ladders Auto-Geradas**

Se a pasta raiz estiver vazia, o servidor gera um conjunto completo de formatos padrão no primeiro boot:

| IDs gerados | Tipo | Observações |
| :--- | :--- | :--- |
| `singles_50_casual`, `singles_100_casual`, `singles_ranked` | Singles | Casual em Nv. 50 e Nv. 100; ranqueada em Nv. 50 |
| `doubles_50_casual`, `doubles_100_casual`, `doubles_ranked` | Doubles | mesmo padrão |
| `triples_50_casual`, `triples_100_casual`, `triples_ranked` | Triples | mesmo padrão |
| `monotype_50_casual`, `monotype_100_casual` | Monotype (Singles) | Cláusula Mesmo Tipo aplicada pelo `monotype` no ID |
| `tourney_solo`, `tourney_duplas`, `tourney_trios`, `tourney_monotype` | Torneio | usadas pelo sistema de torneios, não pela fila normal |

Todo arquivo gerado sai com os **defaults da classe** para as regras, e as ranqueadas / de torneio ainda recebem `"banPresets": ["ou"]` (o torneio monotype recebe `["monotype"]`). Ou seja, um `singles_ranked.json` recém-gerado ainda tem `allowDynamax: true`, `allowRestrictedLegendary: true`, `maxSubLegendary: 6`, etc. — **se você quer um meta ranqueado apertado, precisa apertar isso você mesmo.**

---

## **Template de Configuração**

Abaixo, um template completo de Ladder mostrando todos os campos da classe `Ladder.java`. Os valores aqui são um exemplo ilustrativo de **Singles ranqueado apertado** — não uma cópia do arquivo auto-gerado. Para referência, os defaults da classe (o que você tem se omitir um campo) são: `enforce*Clause: true`, todo `allow*: true`, todo `max*: 6`, `requiredTeamSize: 6`, `adjustLevel: 50`.

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

- **`id`**: ID interno único da categoria (deve coincidir exatamente com o nome do arquivo, sem a extensão `.json`).
- **`queueLabel`**: Nome da fila exibido em pacotes de rede e interfaces (ex.: Fila Ranqueada, Fila Rápida, Torneio).
- **`displayName`**: Título amigável mostrado aos jogadores nos menus do mod.
- **`description`**: Breve descrição explicando as regras ou o propósito da fila na interface.
- **`ranked`** (true/false): Determina se a categoria concede pontos de ranking (Elo/Glicko-2) e atualiza o leaderboard global.

### **2. Regras Estruturais de Combate**

- **`battleTypeId`**: Formato do campo de batalha. Valores suportados:
  - "singles" (1v1)
  - "doubles" (2v2)
  - "triples" (3v3)
- **`requiredTeamSize`**: Quantos Pokémon o jogador precisa trazer para entrar na fila / aceitar um duelo (Padrão: 6). Diminua (ex.: `3`) para um formato estilo "traga 3, use 3".
- **`adjustLevel`**: Nível para o qual todos os Pokémon da equipe são temporariamente ajustados na batalha (Padrão: 50). Use **`0`** para desativar o ajuste e lutar no nível real de cada Pokémon.

### **3. Cláusulas Competitivas**

- **`enforceSpeciesClause`** (true/false): Impede que o jogador use dois ou mais Pokémon da mesma espécie na mesma equipe.
- **`enforceItemClause`** (true/false): Impede que dois ou mais Pokémon segurem o mesmo item equipado.

### **4. Filtros de Meta (Lendários, Míticos e Paradoxos)**

O BattleHUB lê as próprias *labels* do Cobblemon para cada espécie, então esses filtros continuam corretos conforme o Cobblemon adiciona novos Pokémon — você nunca mantém uma lista manual.

**Toggles de permissão** — um `allow* : false` bane a categoria inteira de vez:

- **`allowRestrictedLegendary`** (true/false): lendários *restritos* — os de capa (Mewtwo, Rayquaza, Koraidon…) que o VGC oficial restringe.
- **`allowMythical`** (true/false): Pokémon míticos (Mew, Celebi, Jirachi…).
- **`allowParadox`** (true/false): Pokémon Paradoxo (Great Tusk, Iron Valiant…).

**Limites numéricos** — usados quando o `allow*` correspondente é `true`, para limitar quantos daquela categoria a equipe pode trazer (default da classe `6` = na prática, ilimitado):

- **`maxSubLegendary`**: máximo de Pokémon com a label `legendary` / `sub-legendary` do Cobblemon que **não** estão na lista de restritos (Articuno, Zapdos, Regirock, os gênios…). Não existe toggle `allowSubLegendary`; coloque `0` para bani-los completamente.
- **`maxRestricted`**: máximo de lendários restritos (só importa se `allowRestrictedLegendary` for `true`).
- **`maxMythical`**: máximo de Pokémon míticos.
- **`maxParadox`**: máximo de Pokémon paradoxo.
- **`maxCombinedSpecial`**: máximo **combinado** somando todas as categorias especiais acima — ex.: `1` significa que a equipe pode trazer um lendário *ou* um mítico *ou* um paradoxo, não um de cada.

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