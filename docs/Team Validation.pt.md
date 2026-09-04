# **Validação de Equipes**

---

## **Validação de Equipes**

Para garantir que as partidas competitivas e casuais ocorram de forma justa e sem erros de motor de batalha, o **Cobblemon BattleHUB** utiliza um rigoroso sistema de análise de equipes.

Este validador analisa a party do jogador no momento exato em que ele tenta entrar em uma fila ou aceitar um convite de duelo, barrando imediatamente qualquer irregularidade.

---

## **1. Verificações Estruturais de Sobrevivência**

Antes de analisar as regras do formato, o sistema faz duas checagens básicas de infraestrutura:

* **Tamanho Mínimo da Equipe:** O validador confere se o jogador possui a quantidade de Pokémon exigida pela Ladder (ex: 6 para Singles padrão).
* **Estado de Saúde:** O mod varre a party do jogador para garantir que ele possua **pelo menos um Pokémon saudável** (não desmaiado). Se todos os Pokémon da equipe estiverem desmaiados, a entrada na fila é bloqueada com o aviso: *"Todos os seus Pokémon estão debilitados. Cure seu time antes de entrar em batalha."*

## **2. Cláusulas Competitivas (Regras Ranqueadas)**

Se a Ladder ativa for competitiva (`ranked: true`) ou de torneio (`tourney_`), o validador aplica as seguintes restrições:

### **Cláusula de Espécie (Species Clause)**

Impede que o jogador utilize dois ou mais Pokémon da mesma espécie na equipe.

* O sistema normaliza os IDs das espécies (removendo caracteres especiais e convertendo para minúsculas) para evitar fraudes com formas ou nomes alternativos.

### **Cláusula de Item (Item Clause)**

Garante que cada item equipado na party seja único.

* Se dois Pokémon estiverem segurando o mesmo item (por exemplo, duas Leftovers), o sistema barra a entrada.
* Itens vazios (air ou nenhum item) são ignorados pelo filtro.

### **Lista de Itens Banidos (Banned Items)**

O validador verifica se algum item equipado na equipe está registrado na lista global de banimentos do formato (como os banimentos oficiais da classe VGCRules).

## **3. Filtros de Meta Automáticos (Míticos e Paradoxos)**

Diferente de outros mods onde você precisa cadastrar manualmente centenas de Pokémon nas listas de banimento, o BattleHUB faz uma varredura direta no código do Cobblemon pelas **Labels nativas** do jogo.

Isso garante um bloqueio 100% preciso de qualquer criatura classificada como mythical ou paradox pela própria base do Cobblemon, mantendo a sua configuração sempre atualizada com futuras adições do mod.

## **4. Algoritmo de Validação Monotype**

Se o formato da Ladder possuir o sufixo ou regra monotype, o validador roda uma checagem de **interseção de conjuntos** sobre os tipos da equipe.

Todo Pokémon tem um conjunto de tipos (1 ou 2). O sistema faz a interseção de todos esses conjuntos; a equipe passa no Monotype **só se essa interseção ainda tiver pelo menos um tipo** — ou seja, existe um único tipo que *todos* os Pokémon da equipe compartilham.

**Exemplo:** Charizard `{Fogo, Voador}` + Talonflame `{Fogo, Voador}` + Arcanine `{Fogo}` → o tipo em comum é `{Fogo}` → **válido** (mono-Fogo). Troque Arcanine por Gyarados `{Água, Voador}` e o tipo compartilhado vira `{Voador}` → ainda válido (mono-Voador). Adicione um Pokémon `{Água}` puro e a interseção fica vazia → **rejeitado**.

---
