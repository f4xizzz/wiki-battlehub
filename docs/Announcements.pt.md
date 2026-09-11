# **Anúncios**

---

## **Configuração de Anúncios**

O sistema de anúncios do **Cobblemon BattleHUB** permite que você exiba novidades, avisos importantes e links diretamente nos menus (GUI) do mod para os jogadores. Ele utiliza o formato moderno de texto **MiniMessage**, suportando cores, gradientes e até links clicáveis que abrem o navegador.

---

### **Caminho do Arquivo**

`config/cobblemon_battlehub/announcements.json`

## **Modelo Padrão de Configuração**

Abaixo está o modelo oficial gerado de forma automática pela classe `AnnouncementsManager`:

```json
{
  "version": 1,
  "mensagens": [
    "<gradient:gold:yellow><bold>[NEWS]</bold></gradient> <white> Season 1 has started!</white>",
    "<click:open_url:'https://discord.gg/GbbbNvQG3N'><aqua><u>Join our Discord!</u></aqua></click>"
  ]
}
```

!!! warning "A chave do array é `mensagens`, não `messages`"
    A lista de mensagens no `announcements.json` se chama **`mensagens`** (em português). Se você renomear para `messages`, o mod carrega zero anúncios. Mantenha a chave exatamente como foi gerada.

---

## **Explicação Detalhada dos Parâmetros**

### **1. Controle de Notificação (`version`)**

* **`version`** (Padrão: 1):
  Este inteiro é o rastreador de "não lido". Cada cliente guarda a última versão que já viu em `config/cobblemon_battlehub/announcements_state.txt`. Quando você edita o `announcements.json` e **sobe o número da versão** (ex: 1 → 2), todo jogador ganha um selo de **Novo / não lido** no painel de anúncios até abri-lo. Se você editar o texto mas deixar a versão igual, quem já leu não é notificado de novo.

### **2. Conteúdo Visual (`mensagens`)**

* **`mensagens`** (array de strings):
  As linhas de anúncio exibidas na interface do jogador, de cima para baixo. Adicione quantas quiser. Cada linha é interpretada com **MiniMessage (Kyori)**.

---

## **Como Estilizar e Criar Anúncios Incríveis**

O mod interpreta a sintaxe do **MiniMessage**, então você não precisa se limitar aos códigos de cores legados do Minecraft (como `&a`, `&c`). Veja algumas opções de formatação suportadas:

### **1. Cores e Gradientes**

* **Cores Simples:** use `<red>`, `<blue>`, `<green>`, `<gold>`, etc.
    * *Exemplo:* `<green>Nova atualização disponível!</green>`
* **Gradientes:** crie transições de cores suaves usando `<gradient:cor1:cor2>`.
    * *Exemplo:* `<gradient:gold:yellow><bold>[NOVIDADE]</bold></gradient>`

### **2. Formatação de Texto**

* **Negrito (Bold):** `<bold>` ou `<b>`
* **Sublinhado (Underline):** `<underline>` ou `<u>`
* **Itálico (Italic):** `<italic>` ou `<i>`

### **3. Links Clicáveis (Eventos de Clique)**

Você pode fazer com que um texto abra o navegador do jogador ao ser clicado:

* **Sintaxe do Link:** `<click:open_url:'LINK'>Texto Clicável</click>`
    * *Exemplo Prático:* `<click:open_url:'https://discord.gg/GbbbNvQG3N'><aqua><u>Clique aqui para entrar no Discord!</u></aqua></click>`

!!! note "O que realmente renderiza"
    As linhas de anúncio são convertidas para o formato de texto clássico do Minecraft, então **cores, gradientes e as decorações negrito / sublinhado / itálico funcionam**, e `<click:open_url:'...'>` é suportado como caso especial. Outras tags do MiniMessage — `<hover>`, `<click:run_command>`, `<click:suggest_command>`, `<insert>`, `<font>` — **não** são aplicadas. Use aspas simples na URL (`'...'`); aspas duplas também funcionam, mas escolha um tipo e não misture na mesma tag.

---

## **Como Recarregar as Novas Mensagens**

Após alterar as mensagens ou subir a versão do arquivo `announcements.json`, aplique as novidades instantaneamente ao servidor e sincronize a tela de todos os jogadores online executando:

`/bh reload`

---
