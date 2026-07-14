# **Ban Presets**

---

## **Presets de Banimento**

Para evitar que os administradores de servidores tenham de digitar manualmente longas listas de Pokémon banidos em cada categoria de combate (Ladder), o **Cobblemon BattleHUB** introduz o sistema de **Ban Presets**.

Estes presets são ficheiros JSON simples que armazenam listas pré-definidas de Pokémons. Qualquer Ladder pode carregar um ou mais destes presets simultaneamente apenas referenciando o nome do ficheiro.

### **Caminho do Diretório**

`config/cobblemon_battlehub/ladders/ban_presets/`

## **Como Funciona a Lógica em Cascata (Smogon Tiers)**

O mod gera automaticamente as listas baseadas nas divisões competitivas oficiais da Smogon. Para simplificar a manutenção e evitar ficheiros gigantescos repetitivos, o código do mod utiliza uma **Lógica em Cascata**.

Isso significa que as tiers inferiores importam de forma automática todos os banimentos das tiers que estão acima delas. Veja como funciona o fluxo de herança direta:

        [Ubers (Bans base)]  
             ↓  
        [OU (Ubers + Bans OU)]  
             ↓  
        [UU (OU + Bans UU)]  
             ↓  
        [RU (UU + Bans RU)]  
             ↓  
        [NU (RU + Bans NU)]  
             ↓  
        [PU (NU + Bans PU)]

* Se você utilizar o preset ou, ele banirá os Pokémons banidos de OU **mais** todos os de Ubers.  
* Se você utilizar o preset pu, ele banirá os Pokémons banidos de PU **mais** todos os de NU, RU, UU, OU e Ubers de uma só vez.  
* **Nota:** Os formatos específicos como lc (Little Cup), doubles\_ou e monotype possuem as suas próprias heranças ou listas isoladas adaptadas às suas regras particulares.

---

## **Presets Base Gerados Automaticamente**

Na primeira inicialização do mod, os seguintes ficheiros JSON são gerados de forma automática na pasta de presets:

### **Listas de Categorias Especiais**

| Nome do Preset | Descrição |
| :---- | :---- |
| `legendaries` | Contém todos os Pokémon Lendários e Míticos (ex: Mewtwo, Lugia, Zacian, Koraidon). |
| `ultra_beasts` | Contém todas as Ultra Beasts do ecossistema Pokémon (ex: Nihilego, Buzzwole, Kartana). |
| `paradoxes` | Contém todos os Pokémon Paradoxos do passado e do futuro (ex: Great Tusk, Iron Valiant). |

---

### **Listas de Tiers Oficiais Smogon**

| Nome do Preset | Formato / Referência Smogon |
| :---- | :---- |
| `ubers` | Pokémons banidos da categoria Ubers (Anything Goes, ex: Calyrex-Shadow). |
| `ou` | Pokémons banidos da tier principal Overused (OU) \+ Ubers. |
| `uu` | Pokémons banidos da tier Underused (UU) \+ OU \+ Ubers. |
| `ru` | Pokémons banidos da tier Rarelyused (RU) \+ UU \+ OU \+ Ubers. |
| `nu` | Pokémons banidos da tier Neverused (NU) \+ RU \+ UU \+ OU \+ Ubers. |
| `pu` | Pokémons banidos da tier PU \+ NU \+ RU \+ UU \+ OU \+ Ubers. |
| `lc` | Pokémons banidos do formato Little Cup (Pokémons nível 5 que são fortes demais). |
| `doubles_ou` | Pokémons banidos no formato de Duplas OU (Doubles Ubers). |
| `monotype` | Pokémons banidos especificamente no formato Monotype. |

---

## **Como Criar um Preset de Banimento Customizado**

O sistema do BattleHUB foi programado para carregar **qualquer** ficheiro .json colocado dentro da pasta ban\_presets. Você pode criar regras e formatos exclusivos para eventos do seu servidor em poucos segundos.

### **Passo 1: Criar o arquivo JSON**

Vá até a pasta `ban_presets/` e crie um ficheiro com o nome que desejar. O nome do ficheiro (sem o .json) será o ID do seu preset.

* *Exemplo:* `evento_sem_iniciais.json`

### **Passo 2: Adicionar a Lista de Pokémons**

Adicione os Pokémons desejados em formato de lista (Array JSON). O sistema é inteligente e converte automaticamente os nomes para letras minúsculas, além de ignorar o prefixo cobblemon: caso seja adicionado por engano:

        [  
          "charizard",  
          "blastoise",  
          "venusaur",  
          "cobblemon:meowscarada"  
        ]

### **Passo 3: Utilizar o Preset em uma Ladder**

Abra a configuração da sua Ladder desejada (na pasta `ladders/`) e adicione o ID do seu preset dentro da lista `"banPresets"`:

        {  
          "id": "evento_singles",  
          "displayName": "Combate de Evento",  
          "banPresets": [  
            "evento_sem_iniciais"  
          ]  
        }

### **Passo 4: Recarregar**

Para aplicar e sincronizar os novos arquivos criados, execute o comando de reload no console ou como administrador in-game:

`/bh reload`

---