# **FAQs**

---

## **Perguntas Frequentes**

Bem-vindo à seção de dúvidas frequentes do **Cobblemon BattleHUB**. Abaixo, compilamos as respostas para as perguntas mais comuns da nossa comunidade de administradores e donos de servidores.

### **1\. Onde devo hospedar as imagens para a Loja?**

Para garantir que o carregamento das imagens 2D na interface do mod seja rápido e livre de bloqueios de segurança (CORS), recomendamos fortemente o uso do [**Imgur**](https://imgur.com/).

* **Como fazer:** Faça o upload da sua imagem no Imgur, clique com o botão direito nela e escolha **"Copiar Endereço da Imagem"** (o link deve terminar obrigatoriamente com a extensão `.png` ou `.jpg`).  
* Cole esse link direto no campo imageUrls do seu `shop.json` ou nas configurações do `webhook.json`.  
* **Nota Técnica:** O mod utiliza um sistema próprio que faz o download da imagem apenas uma vez na máquina do jogador e a guarda na memória, garantindo alta performance mesmo com dezenas de itens na loja\!

---

### **2\. Como posso adicionar arenas personalizadas com as construções do meu servidor?**

O Cobblemon BattleHUB instância as arenas físicas no mundo de batalhas utilizando o sistema nativo de **Structures** `(.nbt)` do Minecraft. Para personalizar a aparência das arenas, você não precisa alterar o mod, basta usar um **Datapack**\!

1. Em nosso [Discord](https://discord.gg/aDCgBbvRe5), disponibilizamos o download do pacote de *Structures* base do mod.  
2. Salve as construções `(.nbt)` das suas arenas exclusivas usando blocos de estrutura (*Structure Blocks*) no jogo.  
3. Substitua os arquivos .nbt do pacote original pelos seus, mantendo os mesmos nomes.  
4. Coloque a pasta do datapack dentro de `world/datapacks/` no seu servidor e execute `/reload`.

Na próxima vez que uma batalha for iniciada, o mod construirá a sua arena personalizada\!

---

### **3\. Como faço para adquirir o mod e a licença para o meu servidor?**

O processo de aquisição é seguro, rápido e 100% automatizado\! Para obter a sua cópia do **Cobblemon BattleHUB** e a sua chave de ativação (*License Key*), siga os passos abaixo:

1. Junte-se à nossa comunidade no [Discord](https://discord.gg/aDCgBbvRe5).  
2. Navegue até o canal/seção de **Loja**.  
3. Selecione o pacote do Cobblemon BattleHUB e adicione-o ao seu **carrinho**.  
4. Você será redirecionado para concluir o pagamento de forma segura no site oficial da **Stripe**.  
5. Assim que o pagamento for aprovado, o sistema enviará instantaneamente para você o arquivo `.jar` do mod e a sua **License Key** exclusiva.  
6. Basta colocar o mod no servidor, rodar `/bh activation [SUA_CHAVE]` e aproveitar\!

---

!!! tip "Ainda com Dúvidas?"
    Se a sua dúvida técnica ou de configuração não foi respondida aqui ou nas páginas anteriores da nossa documentação, não hesite em abrir um **Ticket de Suporte** no nosso Discord. Nossa equipe estará pronta para ajudá-lo a configurar o cenário de eSports perfeito para o seu servidor!