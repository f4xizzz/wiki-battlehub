# **Ban Presets**

---

## **Presets de Banimento**

Para evitar que os administradores de servidores tenham de digitar manualmente longas listas de Pokémon banidos em cada categoria de combate (Ladder), o **Cobblemon BattleHUB** introduz o sistema de **Ban Presets**.

Estes presets são ficheiros JSON simples que armazenam listas pré-definidas de Pokémons. Qualquer Ladder pode carregar um ou mais destes presets simultaneamente apenas referenciando o nome do ficheiro.

### **Caminho do Diretório**

`config/cobblemon_battlehub/ladders/ban_presets/`

## **Como Funciona a Lógica em Cascata (Smogon Tiers)**

O mod gera automaticamente as listas baseadas nas divisões competitivas oficiais da Smogon. Para simplificar a manutenção e evitar ficheiros gigantescos repetitivos, o código do mod utiliza uma **Lógica em Cascata**.

Isso significa que os tiers inferiores importam de forma automática todos os banimentos dos tiers acima deles. Veja o fluxo de herança:

```text
ubers   (lista base)
  └─ ou    = ubers + bans de OU
       └─ uu    = ou + bans de UU
            └─ ru    = uu + bans de RU
                 └─ nu    = ru + bans de NU
                      └─ pu    = nu + bans de PU
```

* Se você usar o preset `ou`, ele bane tudo que é banido em OU **mais** todos os de Ubers.
* Se você usar o preset `pu`, ele bane a lista de PU **mais** todos de NU, RU, UU, OU e Ubers de uma vez.
* `monotype` é construído em cima de `ou` (bans de OU + alguns Pokémon quebrados especificamente no Monotype).
* `doubles_ou`, `lc`, `legendaries`, `ultra_beasts` e `paradoxes` são listas **isoladas** — não herdam de nada.

---

## **Presets Base Gerados Automaticamente**

Na primeira inicialização do mod, os seguintes ficheiros JSON são gerados de forma automática na pasta de presets:

### **Listas de Categorias Especiais**

| Nome do Preset | Descrição |
| :---- | :---- |
| `legendaries` | Pokémon lendários e sublendários (Mewtwo, Lugia, Zacian, Koraidon, os gênios, os Tesouros da Ruína…). **Não** inclui Míticos como Mew ou Celebi — bane esses na mão se precisar. |
| `ultra_beasts` | As 11 Ultra Beasts (Nihilego, Buzzwole, Kartana, Naganadel…). |
| `paradoxes` | Todos os 20 Pokémon Paradoxo, passado e futuro (Great Tusk, Iron Valiant, Roaring Moon, Walking Wake…). |

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

Adicione os Pokémon como um array JSON simples de IDs de espécie. Os nomes são convertidos para minúsculas automaticamente e um prefixo `cobblemon:` perdido é removido. Use o estilo com as formas **juntas, sem separadores**, igual às listas que já vêm no mod (`vulpixalola`, `calyrexshadow`, `sneaselhisui`):

```json
[
  "charizard",
  "blastoise",
  "venusaur",
  "cobblemon:meowscarada"
]
```

### **Passo 3: Utilizar o Preset em uma Ladder**

Abra o arquivo da sua Ladder (na pasta `ladders/`) e adicione o ID do preset em `"banPresets"` — você pode listar vários, e eles se somam entre si e com o `bannedSpeciesKeys` da própria ladder:

```json
{
  "id": "evento_singles",
  "displayName": "Combate de Evento",
  "banPresets": ["evento_sem_iniciais", "legendaries"]
}
```

### **Passo 4: Recarregar**

Para aplicar e sincronizar os novos arquivos criados, execute o comando de reload no console ou como administrador in-game:

`/bh reload`

---