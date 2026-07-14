# **Tournament Examples**

---

## **Exemplo e Configuração de Torneios**

O sistema do **Cobblemon BattleHUB** permite que você crie múltiplos perfis de torneios independentes dentro do servidor. Cada torneio é definido por um arquivo .json individual e possui suas próprias regras, limites de participantes, formato de batalha (Ladder) e descrição visual.

---

## **Guia Prático de Criação e Execução de Torneios**

Siga este fluxo rápido para criar e lançar um novo evento no seu servidor Minecraft:

### **Passo 1: Crie o Arquivo de Perfil**

Vá até a pasta `/tournaments/` e crie um arquivo com a extensão .json (ex: `copa_mensal.json`). Cole a estrutura padrão de exemplo e altere os campos com as informações do seu evento.

### **Passo 2: Recarregue os Torneios no Servidor**

Com o arquivo salvo, sincronize todos os dados executando o seguinte comando in-game ou no console:

`/bhubtournament reload`

### **Passo 3: Abra as Inscrições**

Altere o campo `"manualRegistrationOpen"` para true no arquivo JSON e dê `/bhubtournament reload`, ou use a interface administrativa in-game para liberar o botão de inscrição para a comunidade.

### **Passo 4: Sorteie a Tabela e Inicie o Evento**

Assim que o limite de participantes for preenchido (ou no horário agendado com os jogadores online), execute o comando para travar as inscrições e gerar a árvore de eSports:

`/bhubtournament <ID_do_Torneio> roll`

*(Neste momento, o mod gerará a chave de brackets, enviará a tabela inicial ao Discord e colocará as primeiras lutas da Fase 1 em estado de espera).*

---

### **Caminho do Diretório de Perfis**

`config/cobblemon\_battlehub/tournaments/`

---

## **Modelo de Configuração Real**

Na primeira inicialização, o mod gera automaticamente um perfil de teste de exemplo chamado `tournament_ex.json`. Abaixo está a estrutura completa para você usar como base de criação:

        {  
          "tournamentId": "weekly_cup_ou",  
          "name": "Copa Semanal BattleHUB",  
          "description": "O clássico torneio de eSports 6v6 no formato OverUsed (OU)! Prepare a sua melhor equipa, elabore as suas estratégias de switch e prove que você é o mestre das arenas.",  
          "tournamentActive": true,  
          "manualRegistrationOpen": false,  
          "maxParticipants": 16,  
          "minParticipants": 4,  
          "testModeNoMinLimit": false,  
          "ladderName": "Singles OverUsed (OU)",  
          "ladderId": "tourney_solo",  
          "dateText": "Sábado, 18 de Julho às 19:00",  
          "rewardsText": "1.500 Moedas + Shiny Tinkaton com 6 IVs Perfeitos",  
          "showChampion": true,  
          "lastChampion": "F4xizzz"  
        }

---

## **Explicação Detalhada dos Parâmetros**

### **1\. Identificação Técnica**

* **`tournamentId`** (Padrão: "`default`"):  
  O ID técnico e único do torneio. Ele é utilizado internamente no banco de dados para salvar os estados e no comando de gerenciamento administrativo (ex: `/bhubtournament <tournamentId> roll`). **Use letras minúsculas, números e underlines.**

### **2\. Informações Visuais**

* **`name`**: O título amigável e estilizado que aparece destacado para todos os jogadores na interface de torneios.  
* **`description`**: A sinopse ou regras gerais que explicam o formato do torneio na interface do jogador.  
* **`dateText`**: Texto informativo indicando a data e horário agendados para a competição (ex: *"Sábado, 18 de Julho às 19:00"*).  
* **`rewardsText`**: Texto de exibição destacando o prêmio reservado para quem alcançar a primeira colocação.

### **3\. Mecânicas de Inscrição e Lógica**

* **`tournamentActive`** (true/false):  
  Ativa ou desativa a exibição do torneio na interface gráfica dos jogadores. Se definido como false, o torneio fica oculto in-game.  
* **`manualRegistrationOpen`** (true/false):  
  Determina se as inscrições para o torneio estão liberadas para os jogadores normais se registrarem via menu do mod.  
* **`maxParticipants`** (Padrão: 16):  
  A quantidade máxima de jogadores que podem se inscrever na competição. O mod bloqueia novas inscrições instantaneamente ao atingir este valor.  
* **`minParticipants`** (Padrão: 4):  
  A quantidade mínima de jogadores necessária para que o sorteio de chaves (roll) possa ser executado de forma oficial.  
* **`testModeNoMinLimit`** (true/false):  
  Se ativado, ignora a regra de minParticipants. É ideal para testes de desenvolvimento em servidores locais com bots ou poucos jogadores.

### **4\. Integração de Formatos**

* **`ladderName`**: O nome estético e descritivo do formato que os jogadores lerão (ex: *"OverUsed (OU)"*, *"VGC 2026"*).  
* **`ladderId`** (Padrão: `"tourney_solo"`):  
  O ID técnico da Ladder de regras que as arenas instanciadas herdarão (deve bater com os IDs de arquivos .json localizados na pasta ladders/). Isso garante que os duelos do torneio sigam rigorosamente as cláusulas competitivas e proibições de Pokémon/itens que você definiu.

### **5\. Histórico de Campeões**

* **`showChampion`** (true/false):  
  Ativa ou desativa a exibição do último campeão do torneio no menu gráfico dos jogadores.  
* **`lastChampion`**:  
  O nome (nickname) do jogador que venceu a última edição deste torneio. Este campo é **atualizado automaticamente** pelo TournamentManager no momento em que a final é concluída, mas também pode ser modificado manualmente por aqui se você desejar cadastrar um campeão clássico.

  ---
