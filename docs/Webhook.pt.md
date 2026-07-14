# **Webhook**

---

## **Integração de Webhook do Discord **

O **Cobblemon BattleHUB** vem equipado com uma sofisticada ponte de comunicação em tempo real com o Discord. O mod é capaz de notificar sua comunidade sobre o andamento de torneios, convocações de partidas, resultados e coroação de campeões.

Ele também gera e anexa de forma automatizada o gráfico visual do chaveamento em tempo real.

---

### **Caminho do Arquivo**

`config/cobblemon_battlehub/webhook.json`

---

## **Modelo Padrão de Configuração**

Abaixo está a estrutura gerada de forma automática pelo arquivo WebhookConfig.java na primeira inicialização:

        {  
          "enabled": false,  
          "webhookUrl": "COLE_SUA_URL_AQUI",  
          "msgTournamentStart": {  
            "content": "@everyone",  
            "title": "🏆 O Torneio {name} Começou!",  
            "description": "O sorteio de chaves foi realizado para os **{participants}** participantes.\n\n**CHAVEAMENTO INICIAL:**\n{chaveamento}\n\n*Boa sorte a todos os treinadores!*",  
            "colorHex": "#8A2BE2",  
            "thumbnailUrl": "",  
            "imageUrl": ""  
          },  
          "msgMatchCalled": {  
            "content": "",  
            "title": "⚔️ Nova Luta Convocada!",  
            "description": "A partida pelo bloco **{phase}** está prestes a começar!\n\n🔸 **{p1}** vs **{p2}**\n\nAbram os seus menus e preparem as suas equipes!",  
            "colorHex": "#8A2BE2",  
            "thumbnailUrl": "",  
            "imageUrl": ""  
          },  
          "msgMatchWinner": {  
            "content": "",  
            "title": "🔥 Batalha Finalizada!",  
            "description": "**{winner}** derrotou **{loser}** no bloco **{fase}** e avança na tabela do torneio!",  
            "colorHex": "#8A2BE2",  
            "thumbnailUrl": "",  
            "imageUrl": ""  
          },  
          "msgTournamentWinner": {  
           "content": "@everyone",  
            "title": "👑 TEMOS UM CAMPEÃO!",  
            "description": "O grande vencedor do torneio **{name}** é **{winner}**!\n\nParabéns pela vitória e obrigado a todos os que      participaram!",  
            "colorHex": "#8A2BE2",  
            "thumbnailUrl": "",  
            "imageUrl": ""  
          }  
        }

---

## **Variáveis e Placeholders Dinâmicos**

O mod substitui dinamicamente os termos entre chaves {} antes de disparar o pacote para o Discord. Você pode usar estas variáveis no content, title ou description de qualquer mensagem:

| Placeholder | O que ele substitui 
| :---- | :----
| {name} | Nome fantasia do torneio configurado no perfil.
| {participants} | Quantidade de jogadores inscritos na competição.
| {phase} | Identificação do bloco/fase atual da partida. 
| {p1} | Nome do primeiro jogador convocado no bloco.
| {p2} | Nome do segundo jogador convocado no bloco.
| {winner} | Nome do jogador que venceu a partida ou o torneio.
| {loser} | Nome do jogador derrotado que foi eliminado.
| {bracket} | Link dinâmico ou legenda do status do bracket.

---