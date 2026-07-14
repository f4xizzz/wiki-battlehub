# **Server Config**

---

# **Configuração de Anúncios**

O sistema de anúncios do **Cobblemon BattleHUB** permite que você exiba novidades, avisos importantes e links diretamente nos menus (GUI) do mod para os jogadores. Ele utiliza o formato moderno de texto **MiniMessage**, suportando cores, gradientes e até links clicáveis que abrem o navegador.

---

### **Caminho do Ficheiro**

`config/cobblemon_battlehub/announcements.json`

## **Modelo Padrão de Configuração**

Abaixo está o modelo oficial gerado de forma automática pela classe AnnouncementsManager:

        {  
        "version": 1,  
         "mensagens": [  
          "<gradient:gold:yellow><bold>[NEWS]</bold></gradient> <white> Season 1 has started!<white>",  
         "<click:open_url:'https://discord.gg/aDCgBbvRe5'><aqua><u>Join our Discord!</u></aqua></click>"  
         ]  
        }

---

## **Explicação Detalhada dos Parâmetros**

### **1\. O Controle de Notificação (version)**

* **version** (Padrão: 1):  
  Este número inteiro funciona como o controle de notificações pendentes do jogador.  
  Sempre que o jogador entra no servidor, o cliente compara este número com o arquivo local dele (announcements\_state.txt). Se você atualizar o arquivo announcements.json e **subir o número da versão** (por exemplo, de 1 para 2), o jogo identificará que há novidades e exibirá um alerta visual de **Não Lido / Novo** para todos os jogadores.

### **2\. O Conteúdo Visual (mensagens)**

* **mensagens** (Array de Strings):  
  Uma lista contendo as linhas de anúncio que serão exibidas na interface do jogador. Você pode adicionar quantas linhas quiser, utilizando a rica biblioteca de estilização do **MiniMessage (Kyori)**.

---

## **Como Estilizar e Criar Anúncios Incríveis**

O mod interpreta a sintaxe do **MiniMessage**, o que significa que você não precisa se limitar aos códigos de cores legados do Minecraft (como \&a, \&c). Veja algumas opções de formatação suportadas:

### **1\. Cores e Gradientes**

* **Cores Simples:** `Use <red>, <blue>, <green>, <gold>, etc.`
    * *Exemplo:* `<green>Nova atualização disponível!</green>`
* **Gradientes:** `Crie transições de cores suaves usando </gradient:cor1:cor2>.` 
     * *Exemplo:* `<gradient:gold:yellow><bold\>[NOVIDADE]</bold></gradient>`

### **2\. Formatação de Texto**

* **Negrito (Bold):** `<bold> ou <b>`
* **Sublinhado (Underline):**`<underline> ou <u>`
* **Itálico (Italic):**`<italic> ou <i>`

### **3\. Links Clicáveis (Eventos de Clique)**

Você pode fazer com que um texto abra o navegador do jogador se ele for clicado:

* **Sintaxe do Link:**`<click:open_url:'LINK'>Texto Clicável</click>`
    * *Exemplo Prático:* `<click:open_url:'https://discord.gg/aDCgBbvRe5'><aqua><u>Clique aqui para entrar no Discord!</u></aqua>\</click\>`

---

## **Como Recarregar as Novas Mensagens**

Após alterar as mensagens ou subir a versão do arquivo announcements.json, aplique as novidades instantaneamente ao servidor e sincronize a tela de todos os jogadores online executando:

`/bh reload`

---