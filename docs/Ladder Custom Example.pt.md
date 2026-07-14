# **Ladder Custom Example**

---

# **Guia Prático: Criando uma Ladder de Evento (Little Cup)**

O **Cobblemon BattleHUB** permite que você crie formatos de batalhas totalmente exclusivos para eventos temporários, torneios especiais ou novas filas permanentes.

Para este guia, vamos criar uma categoria baseada no clássico formato **`Little Cup (LC)`** da Smogon: apenas Pokémon bebês ou em seu estágio inicial de evolução, ajustados ao **Nível 5**, e com todas as mecânicas apelativas `(Mega, Z-Move, Dynamax e Tera)` completamente desativadas.

---

### **Caminho do Diretório Customizado**

Todas as suas categorias personalizadas devem ser criadas dentro da subpasta de customização para evitar misturar com as filas padrões do mod:

`config/cobblemon_battlehub/ladders/custom_ladders/`

## **Passo 1: Criando o Ficheiro JSON da Ladder**

Vá até a pasta `custom_ladders/`, crie um ficheiro chamado `little_cup_event.json` e adicione a seguinte estrutura:

        {  
          "id": "little_cup_event",  
          "queueLabel": "Event Queue",  
          "displayName": "Little Cup Event",  
          "description": "Apenas Pokémon nível 5! Sem Megas, Z-Moves, Dynamax ou Terastal.",  
          "ranked": false,  
          "battleTypeId": "singles",  
          "requiredTeamSize": 6,  
          "adjustLevel": 5,  
          "enforceSpeciesClause": true,  
          "enforceItemClause": true,  
          "banPresets": [  
            "lc"  
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
          "allowMega": false,  
          "allowZMove": false,  
          "allowDynamax": false,  
          "allowTera": false  
        }

### **O que configuramos aqui?**

1. **id**: Definido como little\_cup\_event (deve ser exatamente igual ao nome do arquivo .json).  
2. **adjustLevel**: Definido como 5\. Todos os Pokémon terão seus níveis rebaixados ou elevados temporariamente para o nível 5 na arena.  
3. **banPresets**: Carregamos o preset "lc" que já vem gerado por padrão no mod. Isso banirá automaticamente Pokémons estágio 1 que são fortes demais para o formato (como Scyther, Gligar, etc.).  
4. **Mecânicas Desativadas**: Definimos todas as propriedades de Gimmicks (allowMega, allowTera, etc.) como false para garantir um combate puramente estratégico de início de geração.

## **Passo 2: Registrando a Fila no Servidor**

Criar o arquivo JSON na pasta de customização faz o mod carregar o arquivo na memória, mas ele **ainda não aparecerá no menu de matchmaking**. Para liberar a fila para os jogadores, precisamos ativá-la no arquivo principal de configuração.

1. Abra o arquivo principal de configuração em:  
   `config/cobblemon_battlehub/server_config.json` 
2. Adicione o ID da sua nova Ladder (`little_cup_event`) na lista correspondente. Como definimos "ranked": false em nosso JSON, vamos adicioná-la na lista de casuais:

          "activeCasualLadders": [  
            "singles_50_casual",  
            "doubles_50_casual",  
            "little_cup_event"  
          ],

!!! tip "Dica de Organização"
    Se você quiser que essa fila valha pontos e ranking no servidor, basta alterar `"ranked": true` dentro do seu arquivo JSON e registrá-la no campo `"activeRankedLadders"` do seu `server_config.json`.

## **Passo 3: Sincronizando as Alterações**

Com os arquivos salvos, você não precisa reiniciar o servidor físico. Acesse o console do servidor ou execute dentro do jogo (como administrador) o comando:

`/bh reload`

Pronto\! A fila **Little Cup Event** aparecerá instantaneamente nos menus dos jogadores com as regras, ajustes de nível e restrições de Pokémon que você acabou de definir.

---