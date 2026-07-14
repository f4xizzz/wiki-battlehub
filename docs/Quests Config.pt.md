# **Quests Config**

---

## **Configuração do Sistema de Missões (quests.json)**

O sistema de missões do **Cobblemon BattleHUB** recompensa a atividade e a fidelidade dos jogadores através de desafios diários, semanais e permanentes. O arquivo quests.json é o núcleo onde todas as missões, metas de conclusão e comandos de recompensa são armazenados e editados.

---

### **Caminho do Arquivo**

`config/cobblemon\_battlehub/quests.json`

---

## **Como o Sistema Funciona**

O comportamento do sistema é orquestrado de forma dinâmica pela classe QuestManager.java a cada login e término de partida:

1. **Geração Automática:** Na primeira inicialização, se o arquivo quests.json não for encontrado, o mod cria o arquivo automaticamente, populando-o com mais de **90 missões pré-definidas**.  
2. **Seleção Randômica Inteligente:** Para as missões diárias e semanais, o mod não entrega o catálogo inteiro para o jogador. Ele seleciona aleatoriamente um subconjunto de IDs com base em uma *seed* gerada pela data atual combinada com a UUID do jogador. Isso garante que cada jogador receba missões diferentes a cada ciclo\!  
3. **Persistência Seguro:** O progresso atual do jogador (quantas etapas concluiu, o que já foi reivindicado e os horários de reset) é salvo de forma assíncrona no banco de dados.

---

## **Modelo Estrutural de Configuração**

Abaixo está a estrutura simplificada do arquivo `quests.json` que demonstra como as missões e parâmetros globais são definidos:

        {  
          "daily_quests_per_player": 2,  
          "weekly_quests_per_player": 5,  
          "daily_quests": [  
            {  
              "id": "daily_01",  
              "type": "PLAY_MATCHES",  
              "title": "Primeiro Passo",  
              "description": "Participe de 1 partida em qualquer fila.",  
              "targetAmount": 1,  
              "typeFilter": null,  
              "rewardDescription": "50 Dinheiros",  
              "rewardCommands": [  
                "eco deposit 50 dollar %player%"  
              ]  
            }  
          ],  
          "weekly_quests": [  
            {  
              "id": "weekly_01",  
              "type": "WIN_RANKED",  
              "title": "Veterano da Arena",  
              "description": "Vença 15 partidas Ranqueadas.",  
              "targetAmount": 15,  
              "typeFilter": null,  
              "rewardDescription": "1000 Pontos de Batalha",  
              "rewardCommands": [  
                "eco deposit 1000 battlepoint %player%"  
              ]  
            }  
          ],  
          "permanent_quests": [  
            {  
              "id": "perm_mono_fire",  
              "type": "MASTER_MONOTYPE",  
              "title": "Mestre das Chamas",  
              "description": "Alcance 100 vitórias usando Monotype Fogo.",  
              "targetAmount": 100,  
              "typeFilter": "fire",  
              "rewardDescription": "Tag Mestre Fogo",  
              "rewardCommands": [  
                "lp user %player% parent add master_fire"  
              ]  
            }  
          ]  
        }

---

## **Explicação Detalhada dos Parâmetros**

### **1\. Parâmetros de Seleção Global**

* **`daily_quests_per_player`** (Padrão: 2):  
  Determina quantas missões diárias serão sorteadas do pool global e atribuídas para cada jogador individualmente a cada 24 horas.  
* **`weekly_quests_per_player`** (Padrão: 5):  
  Determina quantas missões semanais serão sorteadas do pool e atribuídas individualmente a cada 7 dias.

### **2\. Parâmetros das Listas de Missões (`daily_quests, weekly_quests, permanent_quests`)**

Cada objeto de missão inserido nas listas possui as seguintes propriedades obrigatórias:

* **`id`**:  
  O identificador técnico da missão. Ele é usado pelo banco de dados para salvar se o jogador já concluiu ou resgatou o prêmio. **Nunca duplique IDs\!**  
* **`type`**:  
  O gatilho de ação da missão que se conecta com as estatísticas do mod. Deve bater exatamente com os nomes enumerados em QuestType.java (Ex: `PLAY_MATCHES, WIN_RANKED, USE_TYPE, MASTER_MONOTYPE`).  
* **`title`**:  
  O título visual e chamativo que será exibido para o jogador no menu de missões e nos avisos do chat.  
* **`description`**:  
  A explicação de texto que ensina o jogador exatamente o que ele precisa fazer para completar a missão.  
* **`targetAmount`**:  
  A quantidade de vezes que o gatilho precisa ser acionado para a missão ser considerada "completa" (Ex: Vencer **15** partidas, nocautear **50** Pokémons).  
* **`typeFilter`**:  
  Filtro refinador de condições. Ele pode ser usado para limitar a missão a um formato específico (como singles ou doubles) ou a um tipo de Pokémon na equipe (como fire, electric, water). Defina como null se a missão for genérica.  
* **`rewardDescription`**:  
  A descrição visual do prêmio que aparecerá para o jogador quando ele concluir ou resgatar a missão.  
* **`rewardCommands`**:  
  Lista de comandos que o console do servidor executará imediatamente quando o jogador resgatar a recompensa. Suporta a variável `%player%` ou `{player}` para injetar automaticamente o nome do jogador que completou o objetivo.

---

## **Como Recarregar Novas Missões**

Se você adicionar novas missões, alterar descrições ou modificar os prêmios diretamente no arquivo quests.json, você pode aplicar as mudanças em tempo real e sincronizar a interface de todos os jogadores utilizando o comando:

`/bh reload`

---