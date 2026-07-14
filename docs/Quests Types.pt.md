# **Quests Types**

---

## **Tipos de Missões e Gatilhos de Progresso**

O **Cobblemon BattleHUB** vem equipado com um leque variado de objetivos de combate. O comportamento de cada missão é determinado pelo seu type.

Abaixo encontra-se a tabela detalhada de todos os tipos de missões suportados nativamente, como o progresso é calculado e como usar o parâmetro typeFilter em cada cenário.

---

## **Tabela de Gatilhos e Comportamentos (QuestType)**

| Chave Técnica (type) | Filtro (typeFilter) | Comportamento e Progresso |
| :---- | :---- | :---- |
| **`PLAY_MATCHES`** | *Não Utilizado* | Incrementa ![][image1] de progresso por cada partida completada (Ranqueada ou Casual) até ao fim. |
| **`WIN_MATCHES`** | *Não Utilizado* | Incrementa ![][image1] de progresso sempre que o jogador vencer uma partida em qualquer fila. |
| **`WIN_RANKED`** | Formato (ex: singles, doubles) | Incrementa ![][image1] de progresso se o jogador vencer uma partida **Competitiva** que corresponda ao formato do filtro. Se o filtro for null, aceita qualquer formato ranqueado. |
| **`PLAY_RANKED`** | Formato (ex: singles, doubles) | Incrementa ![][image1] de progresso se o jogador completar uma partida **Competitiva** no formato especificado. |
| **`PLAY_CASUAL`** | *Não Utilizado* | Incrementa ![][image1] de progresso ao completar partidas nas filas Casuais do servidor. |
| **`WIN_STREAK`** | *Não Utilizado* | Acumula vitórias consecutivas. Se o jogador vencer, ganha ![][image1] de progresso. **Atenção:** Caso o jogador perca uma partida, o progresso desta missão volta a ser redefinido para ![][image2]\! |
| **`USE_TYPE`** | Tipo Elemental (ex: fire, ghost) | Incrementa ![][image1] de progresso se o jogador completar uma batalha utilizando pelo menos **um** Pokémon do tipo correspondente na sua equipa principal. |
| **`WIN_WITH_ALIVE`** | *Não Utilizado* | Incrementa ![][image1] de progresso sempre que o jogador vencer uma partida (focado em estratégias de sobrevivência). |
| **`WIN_FAST`** | *Não Utilizado* | Incrementa ![][image1] de progresso se o jogador vencer a partida num total de ![][image3] turnos ou menos (onde ![][image3] é o valor definido em targetAmount). |
| **`PLAY_LONG`** | *Não Utilizado* | Incrementa ![][image1] de progresso se a partida demorar ![][image3] turnos ou mais para ser concluída (ou ultrapassar o limite geral de 20 turnos). |
| **`WIN_BY_FORFEIT`** | *Não Utilizado* | Incrementa ![][image1] de progresso se o oponente desistir da partida (render-se/Alt+F4), atribuindo a vitória ao jogador. |
| **`PLAY_NO_FORFEIT`** | *Não Utilizado* | Incrementa ![][image1] de progresso ao completar uma partida inteira sem que ocorram desistências de nenhum dos lados. |
| **`PLAY_MONOTYPE`** | *Não Utilizado* | Incrementa ![][image1] de progresso se o jogador participar de uma batalha com uma equipa que cumpre as regras do Monotype (todos partilham um tipo comum). |
| **`KNOCKOUT_TOTAL`** | *Não Utilizado* | Auxilia no progresso de nocautes de forma equilibrada. Soma ![][image4] pontos se o jogador vencer a partida, ou ![][image1] ponto em caso de derrota, refletindo a agressividade média de combates. |
| **`MASTER_MONOTYPE`** | Tipo Elemental (ex: water, steel) | **Sincronização Direta com o Perfil:** Este tipo monitoriza as vitórias totais acumuladas com equipas Monotype no banco de dados. O mod sincroniza automaticamente o progresso do jogador com o total de vitórias Monotype registadas no seu perfil estatístico global (StatsManager). |

---

## **O Funcionamento do Filtro de Formato**

O parâmetro typeFilter possui um sistema inteligente de validação parcial (formatMatches). Ele verifica se o ID técnico da Ladder jogada contém a palavra especificada no filtro.

### **Exemplo Prático:**

Se você criar uma missão com:

* `type: "WIN_RANKED"`  
* `typeFilter: "doubles"`

O sistema validará o progresso se o jogador vencer na Ladder `doubles_ranked` ou `doubles_50_ranked`, pois ambas as palavras-chave contêm o termo "doubles". Se o filtro for configurado como null ou "", o mod ignora a validação e aceita qualquer formato ranqueado.

---

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABoAAAAZCAYAAAAv3j5gAAAAzklEQVR4XmNgGAUjCsjLyxvKysq6oYtTBSgoKJjLycmVAS05DcT/gexydDV4gaKiojhQ4yxxcXFudDlkALXIF6jWC4i/kmwRUJMk0JCFoqKiPOhy2ADQAuNRi8CAJhYBDRQAGYyMgYlBH0ivlpGRUUGXw2Y5QYtAqQqooBqIZ6HhpUB8H6hxPha5ZHRzCFqEC4BcTvWgwwYGvUVAXIUuhxcQaxFQTQZQ7TMg/o+E3wHxYWVlZTF09RiAWIsoBiALgMERCmSyoMuNglEwNAAA0OVRFOvBHZ8AAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAkAAAAWCAYAAAASEbZeAAAA30lEQVR4XmNgGMRARkaGU15ePgqIZwFxl6KiojqKAiUlJX6gxG4gbhYVFeVRUFAwALKvAXEwXJGcnFw5UOA0kBaEiQH50UB8HWiiOEiBIEgBUPdCuC4gkJWVNQWKfwHSfiAdmkD8Fl0RULMxUPwrELfCObgUgcWBHF8g5z9eRUCGJ0FFxFqnBOQ8x6UIiKsYQIEHZBwA4q1AhRxIilyAYr9ANEwgBogfAQUVoWoYgexmID4Big2wiLGxMStQ0XSg4H6gaQFQBVdB0QPVBAeMQF1qQMUhwKiwA2lEVzCYAAC0J0evUZaWqQAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABEAAAAWCAYAAAAmaHdCAAABMklEQVR4XmNgGAV4gby8vCMQvwbi/yAsJye3Q0ZGhhMmr6KiwqegoLALJg/F65DNgAFGoMQsIP4FxD+B2BJdAVAsCIjXIFuAAoC2CwJtWwik86E2TQEKMyKrAYoVAXE0shgKUFRU1Aca0A9UJAnE14H4CRArIilhAfJng9QhiaECkA1Al6SD2EC6AeqaHJi8lJSUCNSlgghdaACooQ+owBjElpWV1QHy3wPxCSUlJX6QGFDOBsifjKoLCcDCA2QbVAjk9OVA/A8o7gESALkSb3ggewUGQC4AuQTqIidCXgFF7WSgF0zRJYCaYqBhcw1oSCe6PBxg8QocAGNCXB4SUyCD8HrFCYjXAQ3hQpcDAWhMvQViTXQ5kAtcgBJfoLaAMCiVeqOrA8UUKBvgC49RMBgBAJmsUaA7kqF0AAAAAElFTkSuQmCC>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABcAAAAWCAYAAAArdgcFAAABgUlEQVR4Xu2TP0vDUBTFU1QQVBQkBtukadNAR5V8AXEQFxfRqaObq5siCPopRJAOTg5OOouOQp2KiyKC6KabuNT6u22SpheR+GcReuDQ++4997x330sNo4d/Cdd1J+EW3IPr2WzW0ZofIZ/PL2J4WiwWp3zfN4l34Btc0toYiC05iWVZQ7oWoVAoDKI5gS9wRnJs5hE/wbrWx5BRaa6apjmsaxFC82PYIJ6VXC6Xs1k/wFsl7yCNuUA2wHCcMBOuF+h9h/tK2kFa8yTk1PSc03spsa7H+I55qVSaQH8RXkdNHtcIJ5FRxsQsSRHwe2Tbtq9rX21IfQ6+4rltyNfAYtNtf6NJHsI7Xv/gk9qqNo1QLpdH3PYUDV2L4aa4Fs/zRuWEaCtGdA1G6yaq5JoJaTfSmId/oCa8F73kRE989hfmAbpnuBsEwYDkwjd6hNdaHyONOcigW4N1uAEr8Are0DutxTFSmrcgd+84zjyTLMs0pPq0pgtiinCFsF/XevgVPgBM+mcIEeMAbgAAAABJRU5ErkJggg==>