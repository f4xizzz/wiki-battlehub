# **Quests Config**

---

## **Guia Prático: Criando Missões Personalizadas**

O sistema do **Cobblemon BattleHUB** permite que você crie um número ilimitado de desafios para os seus jogadores. Você pode incentivar o uso de tipos esquecidos, recompensar jogadores agressivos ou criar marcas vitalícias de prestígio.

Abaixo, veremos o passo a passo de como estruturar e registrar três exemplos práticos de missões personalizadas diretamente no seu arquivo quests.json.

---

## **Exemplo 1: Missão Diária de Tipo**

Neste exemplo, criaremos uma missão diária simples que incentiva o jogador a testar um Pokémon do tipo Planta na sua equipe.

* **Objetivo:** Participar de 1 batalha usando pelo menos um Pokémon do tipo Grama na equipe.  
* **Recompensa:** $100 em dinheiro no servidor.

### **Estrutura do JSON:**

Adicione este bloco dentro da lista `"daily_quests"` no seu arquivo `quests.json`:

        {  
          "id": "daily_custom_grass",  
          "type": "USE_TYPE",  
          "title": "Raízes Fortes",  
          "description": "Complete 1 batalha utilizando pelo menos um Pokémon do tipo Planta na sua equipe principal.",  
          "targetAmount": 1,  
          "typeFilter": "grass",  
          "rewardDescription": "100 Moedas",  
          "rewardCommands": [  
            "eco deposit 100 dollar %player%"  
          ]  
        }

---

## **Exemplo 2: Missão Semanal de Combate Rápido**

Neste desafio semanal, recompensamos jogadores que conseguem fechar partidas de forma extremamente rápida e agressiva nas filas competitivas.

* **Objetivo:** Vencer 3 partidas ranqueadas em 10 turnos ou menos.  
* **Recompensa:** 500 Pontos de Batalha (BP).

### **Estrutura do JSON:**

Adicione este bloco dentro da lista `"weekly_quests"` no seu arquivo `quests.json`:

        {  
          "id": "weekly_custom_fast_win",  
          "type": "WIN_FAST",  
          "title": "Ataque Relâmpago",  
          "description": "Vença 3 partidas de arena em um limite máximo de 10 turnos por partida.",  
          "targetAmount": 3,  
          "typeFilter": null,  
          "rewardDescription": "500 Pontos de Batalha",  
          "rewardCommands":[  
            "eco deposit 500 battlepoint %player%"  
          ]  
        }

---

## **Exemplo 3: Missão Permanente de Maestria (Mestre dos Dragões)**

As missões permanentes funcionam como conquistas vitais (*Achievements*). Elas não resetam no ciclo diário ou semanal. Neste exemplo, vamos criar um marco para quem atingir 150 vitórias jogando com equipes Monotype Dragão.

* **Objetivo:** Alcançar 150 vitórias com equipe Monotype Dragão.  
* **Recompensa:** Atribuição de uma Tag exclusiva via LuckPerms e anúncio global.

### **Estrutura do JSON:**

Adicione este bloco dentro da lista `"permanent_quests"` no seu arquivo `quests.json`:

    {  
      "id": "perm_custom_dragon_master",  
      "type": "MASTER\_MONOTYPE",  
      "title": "Soberano dos Dragões",  
      "description": "Alcance a marca histórica de 150 vitórias competitivas utilizando uma equipe Monotype Dragão.",  
      "targetAmount": 150,  
      "typeFilter": "dragon",  
      "rewardDescription": "Tag Lendária [Mestre Dragão]",  
      "rewardCommands": [  
        "lp user %player% parent add mestre_dragao",  
        "broadcast &6[BattleHUB] &e%player% alcançou a maestria máxima e agora é um Mestre dos Dragões!"  
      ]  
    }

---

## **Boas Práticas ao Criar Novas Missões**

Para garantir a estabilidade do sistema e evitar problemas de compatibilidade nos salvamentos do banco de dados, certifique-se de seguir estas três regras simples:

1. **IDs Únicos e Padronizados:** Nunca utilize o mesmo ID para duas missões diferentes. Recomendamos utilizar um prefixo claro para cada lista, como `daily_...`, `weekly_...` ou `perm_...`.  
2. **Normalização dos Filtros:** O parâmetro typeFilter faz a leitura direta dos tipos elementais do Cobblemon e dos IDs de formato. Insira sempre em **letras minúsculas** (ex: fire em vez de Fire, doubles em vez de Doubles).  
3. **Sintaxe dos Comandos:** Você pode encadear quantos comandos de console desejar na lista `"rewardCommands"`. O sistema substituirá automaticamente as variáveis `%player%` e `{player}` pelo nick atualizado do jogador.

---

## **Como Aplicar as Novas Missões**

Após salvar as modificações no arquivo quests.json, aplique as alterações instantaneamente aos jogadores ativos no servidor executando o comando de recarga:

`/bh reload`

---