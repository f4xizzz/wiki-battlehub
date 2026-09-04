# **Ladder Custom Example**

---

## **Guia Prático: Criando uma Ladder de Evento (Little Cup)**

O **Cobblemon BattleHUB** permite que você crie formatos de batalha totalmente exclusivos para eventos temporários, torneios especiais ou novas filas permanentes.

Neste guia vamos criar uma categoria baseada no clássico **Little Cup (LC)** da Smogon: apenas Pokémon não evoluídos, tudo ajustado para o **Nível 5**, e com todas as mecânicas de poder (Mega, Z-Move, Dynamax, Tera) desativadas.

---

### **Caminho do Diretório Customizado**

Crie as categorias personalizadas dentro da subpasta de customização para não misturar com as filas padrão do mod:

`config/cobblemon_battlehub/ladders/custom_ladders/`

## **Passo 1: Criar o Arquivo JSON da Ladder**

Na pasta `custom_ladders/`, crie o arquivo `little_cup_event.json`:

```json
{
  "id": "little_cup_event",
  "queueLabel": "Event Queue",
  "displayName": "Little Cup Event",
  "description": "Only Level 5 Pokémon! No Megas, Z-Moves, Dynamax, or Terastal.",
  "ranked": false,

  "battleTypeId": "singles",
  "requiredTeamSize": 6,
  "adjustLevel": 5,

  "enforceSpeciesClause": true,
  "enforceItemClause": true,

  "banPresets": ["lc"],
  "bannedSpeciesKeys": ["ditto", "arceus"],
  "bannedItemKeys": ["choice_band"],
  "bannedAbilityKeys": ["sturdy"],
  "bannedMoveKeys": ["swords_dance"],

  "allowRestrictedLegendary": false,
  "allowMythical": false,
  "allowParadox": false,
  "allowMega": false,
  "allowZMove": false,
  "allowDynamax": false,
  "allowTera": false,

  "maxSubLegendary": 0,
  "maxRestricted": 0,
  "maxMythical": 0,
  "maxParadox": 0,
  "maxCombinedSpecial": 0
}
```

!!! warning "Tem que ser JSON válido"
    Toda entrada precisa de vírgula no final **menos a última antes de um `}`**, e não tem vírgula depois do `}` final. Uma vírgula faltando e o arquivo inteiro deixa de carregar. Repare que o campo é **`allowRestrictedLegendary`** — `allowRestrictedPokemon` (o nome interno do preset) é ignorado aqui.

### **O que configuramos aqui?**

1. **`id`** — `little_cup_event`, tem que ser idêntico ao nome do arquivo.
2. **`adjustLevel`** — `5`; todo Pokémon é ajustado temporariamente para o nível 5 na batalha.
3. **`banPresets`** — carrega o preset `lc` que já vem no mod, que bane pra você os Pokémon "inviáveis no LC" (veja [Ban Presets](Ban Presets.md)). As listas diretas `bannedSpeciesKeys` / `bannedItemKeys` / `bannedAbilityKeys` / `bannedMoveKeys` são somadas por cima; use IDs em minúsculo com underscore (`swords_dance`), o prefixo `cobblemon:` é opcional.
4. **Mecânicas desativadas** — todos os `allow*` de gimmick em `false`, e os limites `max*` em `0`, então nenhum lendário / mítico / paradoxo entra.

## **Passo 2: Registrar a Fila no Server Config**

Colocar o JSON na pasta custom carrega ele na memória, mas ele **não aparece no menu de matchmaking** até você listar no server config.

1. Abra `config/cobblemon_battlehub/server_config.json`.
2. Adicione o ID da Ladder na lista certa. Definimos `"ranked": false`, então vai na lista de casuais:

```json
"activeCasualLadders": [
  "singles_50_casual",
  "doubles_50_casual",
  "little_cup_event"
]
```

!!! tip "Deixar ranqueada"
    Para essa fila valer rating e aparecer no leaderboard, coloque `"ranked": true` no arquivo da Ladder e liste o ID em `"activeRankedLadders"`.

## **Passo 3: Sincronizar as Alterações**

Não precisa reiniciar o servidor. No console ou dentro do jogo como admin:

`/bh reload`

A fila **Little Cup Event** aparece na hora nos menus dos jogadores com as regras, o ajuste de nível e as restrições que você definiu.

---
