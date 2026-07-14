# **Server Config**

---

## **Configuração Geral**

O arquivo `server_config.json` é o núcleo de configuração do **Cobblemon BattleHUB**. Ele é gerado automaticamente na primeira inicialização do mod dentro do diretório do seu servidor.

### **Caminho do Arquivo**

`config/cobblemon_battlehub/server_config.json`

## **Modelo Padrão de Configuração**

Abaixo está o modelo com a estrutura oficial gerada pela classe ServerConfig para você usar como referência rápida:

        {  
          "activeRankedLadders": [  
            "singles_ranked",  
           "doubles_ranked",  
           "triples_ranked"  
          ],  
         "activeCasualLadders": [  
          "singles_50_casual",  
           "doubles_50_casual"  
        ],  
         "actionTimerSeconds": 30,  
         "afkStrikeLimit": 3,  
         "afkExtensionSeconds": 15,  
         "maxAfkAutopilotRounds": 3,  
         "autopilotActionSeconds": 5,  
         "currentSeasonNumber": 1,  
         "currentSeasonId": "season_1",  
          "currentSeasonName": "Season 1",  
         "currentSeasonStartedAtMs": 1718064000000,  
         "completedRankedSeasons": [],  
         "rewards": {  
          "milestoneRewardsEnabled": true,  
           "seasonRewardsEnabled": true,  
           "milestoneRewards": [  
            {  
             "id": "ranked_wins_10",  
             "title": "10 Ranked Wins",  
             "statType": "RANKED_WINS",  
                "threshold": 10,  
             "commands": [  
                  "give %player% minecraft:diamond 5"  
                ]  
              },  
           {  
             "id": "ranked_wins_25",  
             "title": "25 Ranked Wins",  
             "statType": "RANKED_WINS",  
             "threshold": 25,  
             "commands": [  
               "give %player% minecraft:netherite_ingot 1"  
             ]  
           },  
           {  
             "id": "arena_matches_50",  
             "title": "50 Arena Matches",  
             "statType": "TOTAL_BATTLES",  
             "threshold": 50,  
             "commands": [  
               "eco give %player% 1000"  
             ]  
           }  
         ],  
         "seasonEndRewards": {  
           "placementRewards": [  
             {  
               "maxPlacement": 1,  
               "minimumGames": 5,  
                "commands": [  
                 "lp user %player% parent add top1"  
                  ]  
               },  
              {  
                "maxPlacement": 3,  
                "minimumGames": 10,  
                "commands": [  
                 "lp user %player% parent add top3"  
                  ]  
             },  
             {  
                  "maxPlacement": 10,  
                  "minimumGames": 20,  
                "commands": [  
                   "eco give %player% 5000"  
                ]  
              }  
              ],  
              "participationReward": {  
               "minimumGames": 15,  
               "commands": [  
                "give %player% cobblemon:poke_ball 15"  
               ]  
              }  
          }  
         }  
        }

---

## **Explicação Detalhada dos Parâmetros**

### **1. Categorias Ativas (`Ladders`)**

* **`activeRankedLadders`**: Lista contendo os IDs das categorias competitivas (Ranked) que estarão disponíveis nas filas do servidor.  
* **`activeCasualLadders`**: Lista contendo os IDs das categorias sem valer pontos (Casual) liberadas para matchmaking rápido.

---

### **2. Timers e Sistema Anti-AFK**

* **`actionTimerSeconds`** (Padrão: 30): Tempo máximo (em segundos) que um jogador tem para tomar uma decisão em seu turno de batalha.  
* **`afkStrikeLimit`** (Padrão: 3): Quantidade máxima de vezes que o jogador pode estourar o tempo de turno antes de sofrer punições.  
* **`afkExtensionSeconds`** (Padrão: 15): Tempo adicional em segundos que o sistema concede em situações específicas de travamento ou reconexão.  
* **`maxAfkAutopilotRounds`** (Padrão: 3): Limite de turnos seguidos que o sistema de piloto automático pode agir de forma automática antes do jogador ser removido da partida por AFK.  
* **`autopilotActionSeconds`** (Padrão: 5): Tempo de resposta do piloto automático após o cronômetro do jogador esgotar de vez.

---

### **3. Temporadas (Seasons)**

* **`currentSeasonNumber`**: Contador incremental identificando o número ordinal da temporada atual.  
* **`currentSeasonId`**: Identificador interno gerado automaticamente pela lógica do mod para banco de dados (ex: season\_1).  
* **`currentSeasonName`**: Nome fantasia/exibição da temporada ativa (ex: Season 1).  
* **`currentSeasonStartedAtMs`**: Timestamp Unix em milissegundos marcando o início exato da temporada em andamento.  
* **`completedRankedSeasons`**: Histórico que armazena dados de todas as temporadas finalizadas e arquivadas pelo comando /bh season rollover.

---

### **4. Sistema de Premiações (rewards)**

Permite automatizar o envio de itens, moedas ou permissões via console quando metas forem alcançadas.


#### **Configurações Globais**

* **`milestoneRewardsEnabled`**: (true/false) Habilita ou desabilita as recompensas por marcos de progresso do jogador.  
* **`seasonRewardsEnabled`**: (true/false) Habilita ou desativa as recompensas distribuídas aos melhores colocados no encerramento de temporada.

#### **Metas de Progresso (`milestoneRewards`)**

Array onde você cadastra metas específicas de jogo:

* `id`: String única para registrar a entrega da meta e evitar resgates duplicados.  
* `title`: O nome visual exibido aos jogadores.  
* `statType`: Tipo de estatística avaliada no gatilho. As opções suportadas na classe Java são:  
  * `TOTAL_BATTLES`: Quantidade total de duelos travados (Ranked + Casual).  
  * `RANKED_WINS`: Total de vitórias na fila competitiva.  
  * `RANKED_MATCHES`: Quantidade de partidas jogadas na fila competitiva.  
  * `QUICK_WINS`: Vitórias em partidas casuais.  
  * `QUICK_MATCHES`: Partidas casuais completadas.  
* `threshold`: O número alvo de vitórias ou partidas para destravar a recompensa.  
* `commands`: Lista de comandos que o console rodará. O marcador %player% substitui automaticamente pelo nick do jogador.

#### **Premiações de Fim de Temporada (seasonEndRewards)**

* **`placementRewards`**: Lista contendo prêmios para o topo do ranking.  
  * `maxPlacement`: Posição máxima elegível (ex: 1 dará apenas para o Top 1, 3 premiará o Top 2 e Top 3, etc).  
  * `minimumGames`: Quantidade mínima de partidas completadas na temporada para evitar que alguém que jogou só uma partida herde o Top 1\.  
  * `commands`: Lista de comandos a serem disparados pelo console para os vencedores.  
* **`participationReward`**: Prêmio de consolação por participação.  
  * `minimumGames`: Mínimo de partidas que o jogador precisa ter completado para ser elegível a este prêmio de participação.  
  * `commands`: Comandos enviados ao console para premiar os participantes válidos.

  ---