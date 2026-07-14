# **Chat Config**

---

## **Como Configurar Passo a Passo**

1. **Garanta a Instalação:** Certifique-se de que o **CarbonChat** está instalado e funcionando perfeitamente em seu ambiente Fabric.  
2. **Sincronize os IDs:** Abra o arquivo de configuração de canais do CarbonChat e verifique os IDs do seu canal global e local. Substitua-os em globalChannelId e localChannelId no arquivo chatconfig.json.  
3. **Personalize Esteticamente:** Modifique as variáveis tabGlobalName e tabLocalName se preferir que elas tenham nomes estilizados como [GLOBAL] ou [ARENA].  
4. **Recarregue no Servidor:** Para aplicar as alterações de chat em tempo real sem precisar reiniciar, execute o comando:  
   `/bh reload`

---

### **Caminho do Arquivo**

`config/cobblemon_battlehub/chatconfig.json`

!!! info "Compatibilidade de Mods de Chat"
    Atualmente, o **Cobblemon BattleHUB** possui suporte nativo e exclusivo para o mod **CarbonChat**.

    Se o seu servidor utiliza outro mod ou sistema de gerenciamento de chat (como *EssentialCmds*, *LPC*, etc.), não se preocupe! Você pode entrar em nosso [Discord Oficial](https://discord.gg/aDCgBbvRe5) e abrir um ticket de atendimento. Nós faremos a integração personalizada para o sistema de chat do seu servidor com o maior prazer.

--- 

## **Modelo Padrão de Configuração**

Abaixo está o modelo padrão gerado pela classe ChatConfig na primeira inicialização:

        {  
         "globalChannelId": "global",  
         "localChannelId": "local",  
         "globalCommand": "g",  
         "localCommand": "l",  
         "tabGlobalName": "GLOBAL",  
         "tabLocalName": "LOCAL"  
    }

---

## **Explicação Detalhada dos Parâmetros**

### **1\. Identificadores do Servidor (CarbonChat)**

Estes parâmetros devem bater exatamente com as configurações de canais (IDs) criadas dentro das configurações do seu CarbonChat para que a comunicação do BattleHUB flua corretamente.

* **globalChannelId** (Padrão: "global"): ID interno do canal de chat global utilizado pelo CarbonChat no servidor.  
* **localChannelId** (Padrão: "local"): ID interno do canal de chat local utilizado pelo CarbonChat (geralmente usado para chats de arenas específicas).

### **2\. Comandos do Cliente (Client-Side)**

Estes são os atalhos que os jogadores usam no chat para alternar rapidamente entre as abas e os canais de conversação integrados.

* **globalCommand** (Padrão: "g"): O comando rápido digitado no chat para alternar para o canal global (ex: /g <mensagem>).  
* **localCommand** (Padrão: "l"): O comando rápido digitado no chat para alternar para o canal local da sua arena (ex: /l <mensagem>).

### **3\. Personalização Visual das Abas (GUI)**

Estes valores alteram a parte estética das abas que aparecem para o jogador na interface de chat do próprio menu do mod (HUD de batalha).

* **tabGlobalName** (Padrão: "GLOBAL"): Nome de exibição que os jogadores verão na aba do canal global na interface do mod.  
* **tabLocalName** (Padrão: "LOCAL"): Nome de exibição que os jogadores verão na aba do canal local na interface do mod.

---
