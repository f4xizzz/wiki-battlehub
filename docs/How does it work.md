# **How does it work?**

---

## **Tournament System Workflow**

The **Cobblemon BattleHUB** tournament system is fully automated and integrated with the server's battle ecosystem. It manages the entire competition lifecycle — from registration opening to crowning the champion — without administrators needing to manually handle external brackets.

---

## **The Tournament Lifecycle**

A tournament execution flow passes through 6 well-defined logical states, controlled by the manager's `Status` property:

[UPCOMING] ➔ [REGISTRATION] ➔ [SEEDING] ➔ [IN_PROGRESS] ➔ [FINISHED]

### **1. Initialization and Profiles (UPCOMING)**

The mod loads all tournament definitions and structural rules directly from the profiles folder. The tournament stays on standby, displaying reward details, scheduled date, and format rules in the player's UI.

### **2. Registration Opening (REGISTRATION)**

When the `manualRegistrationOpen` parameter is set to true (or activated via command), players gain access to register for the tournament through the in-game interface.

* The mod strictly limits registrations to the maximum defined by `maxParticipants`.  
* Each participant's registration data is saved by associating their UUID with their nickname in the database.

### **3. Bracket Drawing and Pairing (SEEDING)**

When the command `/bh tournament <ID> roll` is executed, the system closes registrations and starts building the single elimination bracket.

### **4\. Convocação de Partidas (IN\_PROGRESS)**

Assim que o chaveamento é gerado, o torneio entra em andamento. O sistema de ticks monitora ativamente as partidas e gerencia os combates através de "Blocos de Luta":

1. **Chamada de Partida:** O administrador ou o sistema envia a ordem de início para um bloco. Os dois competidores recebem um alerta global e um temporizador de preparação é iniciado.  
2. **Boss Bar de Transmissão:** Uma Boss Bar global é exibida no topo da tela para todos os jogadores online do servidor que não estão lutando, indicando o progresso atual do torneio (ex: *"Torneio Mensal \- Fase 1"*).

### **5\. Execução Automática de Batalhas**

* The called players must prepare their team and click "Ready" in the mod interface.  
* Once both are ready (or if the preparation time is forced), the mod instantly starts the Cobblemon match using the tournament profile's configured Ladder (e.g. `tourney_solo`).  

### **6. Dynamic Advancement and Completion (FINISHED)**

* Once the battle session ends, the mod intercepts the in-game result.  
* The winner is instantly advanced in the bracket to create the next phase block (e.g. `_phase_2_1` or `_final_1`).  
* The loser is eliminated from the competition.  
* The process repeats until only one active competitor remains. When the final winner is declared, the status changes to FINISHED and the champion's name is permanently recorded in the history (`lastChampion`).

## **Anti-W.O. Mechanism for Preparation Time**

To prevent absent players (AFK) from blocking the progress of the entire tournament, the mod strictly monitors the preparation timeout:

* Each called match receives a deadline in milliseconds.  
* If the timer expires and one of the competitors has not confirmed readiness, the system executes automatic elimination:  
  * If Player 2 marked ready but Player 1 did not, Player 2 is declared the winner by W.O.  
  * If neither player marked ready, the system eliminates both or advances the one with the better seed/readiness.  
  * The disqualification announcement is sent to chat and the bracket is immediately updated in the database.

---
