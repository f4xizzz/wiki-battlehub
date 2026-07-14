# **Commands:**

---

### **Important**
* `/fixbattle`    
*(Creates a request to your opponent asking to cancel the match due to bugs (neither you nor they lose points))*
    * `/fixbattle confirm`   
    *(accepts the opponent's match cancel request)*

---

### **General Commands**

| Command | Description | Permission |
| :--- | :--- | :--- |
| `/battlehub`<br>_Aliases: `/bh`, `/bhub`, `/cbh`_ | Opens the main Cobblemon BattleHUB interface. | `battlehub.base` |
| `/bh status` | Shows the current server status (e.g. players in queue, season, active matches, etc.). | `battlehub.status` |
| `/bh stats` | Shows your personal stats (e.g. rank, points, matches played, etc.). | `battlehub.stats.self` |
| `/bh stats [player]` | Shows the stats of a specific player. | `battlehub.stats.other` |
| `/bh leavequeue` | Removes you from the matchmaking queue. | `battlehub.leavequeue` |
| `/bh leave` | Removes you from an active match or spectator mode. | `battlehub.leave` |
| `/bh spectate` | Opens the spectator interface to watch active matches. | `battlehub.spectate` |
| `/bh quests` | Opens the interface with your daily and monthly quests. | `battlehub.quests` |
| `/bh season` | Shows information about the current season. | `battlehub.season.info` |
| `/bh season stats` | Shows internal stats and data for the active season. | `battlehub.season.status` |
| `/bh season rollover` | Ends the current season and immediately starts the next one. | `battlehub.season.rollover` |
| `/battlehub pokemonstats`<br>_Alias: `/bh pokemonstats`_ | Shows the ranking of the most used Pokémon in ranked matches. | `battlehub.pokemonstats` |
{ .md-typeset__table }

---

### **Administration and Management Commands**

| Command | Description | Permission |
| :--- | :--- | :--- |
| `/battlehub`<br>_Aliases: `/bh`, `/bhub`, `/cbh`_ | Opens the main Cobblemon BattleHUB interface. | `battlehub.base` |
| `/bh serverstatus` | Shows general server information and performance data. | `battlehub.serverstatus` |
| `/bh reload` | Instantly reloads all mod configuration files. | `battlehub.reload` |
| `/bh clearqueue` | Clears the global matchmaking queue, removing all players from it. | `battlehub.clearqueue` |
| `/bh activation [LICENSE-KEY]` | Validates and activates the mod license for the server IP. | `battlehub.activation` |
| `/bh arenas` | Shows the status and technical data of loaded arenas (structures). | `battlehub.arenas` |
| `/bh battles` | Displays a list of all real-time duels currently happening. | `battlehub.battles` |
| `/bh end [player]` | Forces immediate termination of the specified player's match. | `battlehub.end` |
| `/bh clearban [player]` | Removes all restrictions and bans applied to the player. | `battlehub.clearban` |
| `/bh resetrank [player]` | Completely resets a player's ranking points and rank. | `battlehub.resetladder` |
| `/bh adminstats [player]` | Shows a complete report of internal information for player moderation. | `battlehub.adminstats` |
| `/bh forcestart [player1] [player2] [LadderID]` | Forces an immediate match start between two players in a specific category. | `battlehub.forcestart` |
| `/bh leaderboard [LadderID] [Limit]` | Displays the leaderboard for the specified category (Ladder) in chat. | `battlehub.leaderboard` |
{ .md-typeset__table }

---

### **Owner/Director Commands**

| Command | Description | Permission |
| :--- | :--- | :--- |
| `/battlehub`<br>_Aliases: `/bh`, `/bhub`, `/cbh`_ | Opens the main Cobblemon BattleHUB interface. | `battlehub.base` |
| `/bh resetlimitshop [player]` | Refreshes and resets all shop purchase limits for the specified player. | `battlehub.resetlimitshop` |
| `/bh rollpokemonshop` | Resets and forces a new rotation of Pokémon available for purchase in the shop. | `battlehub.rollpokemonshop` |
| `/bh structures list` | Displays a list of all currently generated and active arenas (structures). | `battlehub.structures.list` |
| `/bh structures clear` | Removes and destroys all currently spawned arenas (structures). | `battlehub.structures.clear` |
| `/bh structures tp [ArenaID]` | Instantly teleports you to the coordinates of the arena indicated by the ID. | `battlehub.structures.tp` |
| `/bh structures build [all/ArenaID]` | Builds a specific arena (by ID) or forces generation of all arenas at once. | `battlehub.structures.build` |
{ .md-typeset__table }

---

  <a href="../Tournament%20Commands/" style="text-decoration: none; color: inherit; display: block;">
    <div style="border: 1px solid var(--md-default-fg-color--lightest); border-radius: 8px; padding: 20px; background-color: var(--md-code-bg-color); transition: border-color 0.25s, transform 0.25s, box-shadow 0.25s; height: 100%; display: flex; flex-direction: column; justify-content: space-between;" 
         onmouseover="this.style.borderColor='var(--md-accent-fg-color)'; this.style.boxShadow='0 4px 12px rgba(0,0,0,0.15)'; this.style.transform='translateY(-4px)';" 
         onmouseout="this.style.borderColor='var(--md-default-fg-color--lightest)'; this.style.boxShadow='none'; this.style.transform='none';">
      <div>
        <div style="font-weight: 700; font-size: 1.1rem; color: var(--md-default-fg-color); display: flex; align-items: center; justify-content: space-between; margin-bottom: 8px;">
          <span>🏆 Tournament Commands</span>
          <span style="color: var(--md-accent-fg-color); font-size: 1.2rem;">&rarr;</span>
        </div>
        <div style="font-size: 0.9rem; color: var(--md-default-fg-color--light); line-height: 1.5;">
          Click to go to the page.
        </div>
      </div>
    </div>
  </a>