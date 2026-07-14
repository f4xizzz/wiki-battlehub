# **Ranks**

---

## **Validação de Equipes**

Para garantir que as partidas competitivas e casuais ocorram de forma justa e sem erros de motor de batalha, o **Cobblemon BattleHUB** utiliza um rigoroso sistema de análise de equipes.

Este validador analisa a party do jogador no momento exato em que ele tenta entrar em uma fila ou aceitar um convite de duelo, barrando imediatamente qualquer irregularidade.

---

## **1\. Verificações Estruturais de Sobrevivência**

Antes de analisar as regras do formato, o sistema faz duas checagens básicas de infraestrutura:

* **Tamanho Mínimo da Equipa:** O validador confere se o jogador possui a quantidade de Pokémon exigida pela Ladder (ex: 6 para Singles padrão).  
* **Estado de Saúde:** O mod varre a party do jogador para garantir que ele possua **pelo menos um Pokémon saudável** (não desmaiado). Se todos os Pokémon da equipa estiverem desmaiados, a entrada na fila é bloqueada com o aviso:*"Todos os seus Pokémon estão debilitados. Cure seu time antes de entrar em batalha."*

## **2\. Cláusulas Competitivas (Regras Ranqueadas)**

Se a Ladder ativa for competitiva `(ranked: true)` ou de torneio `(tourney_)`, o validador aplica as seguintes restrições:

### **Cláusula de Espécie `(Species Clause)`**

Impede que o jogador utilize dois ou mais Pokémon da mesma espécie na equipa.

* O sistema normaliza os IDs das espécies (removendo caracteres especiais e convertendo para minúsculas) para evitar fraudes com formas ou nomes alternativos.

### **Cláusula de Item `(Item Clause)`**

Garante que cada item equipado na party seja único.

* Se dois Pokémon estiverem segurando o mesmo item (por exemplo, duas Leftovers), o sistema barra a entrada.  
* Itens vazios (air ou nenhum item) são ignorados pelo filtro.

### **Lista de Itens Banidos `(Banned Items)`**

O validador verifica se algum item equipado na equipa está registrado na lista global de banimentos do formato (como os banimentos oficiais da classe VGCRules).

## **3\. Filtros de Meta Automáticos (Míticos e Paradoxos)**

Diferente de outros mods onde você precisa cadastrar manualmente centenas de Pokémon nas listas de banimento, o BattleHUB faz uma varredura direta no código do Cobblemon pelas **Labels nativas** do jogo.

Isso garante um bloqueio 100% preciso de qualquer criatura classificada como mythical ou paradox pela própria base do Cobblemon, mantendo a sua configuração sempre atualizada com futuras adições do mod.

## **4\. Algoritmo de Validação Monotype**

Se o formato da Ladder possuir o sufixo ou regra monotype, o validador executa um algoritmo matemático de **Interseção de Conjuntos**.

Para que a equipa seja considerada válida para Monotype, o conjunto resultante da interseção de todos os tipos dos Pokémon da equipa não pode ser vazio. Ou seja:

![][image1]

Onde:

* ![][image2] é a quantidade de Pokémon na party.  
* ![][image3] representa o conjunto de tipos do Pokémon ![][image4] (que pode ter 1 ou 2 tipos).

---

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAmwAAABUCAYAAAA/I2vMAAAFCklEQVR4Xu3dT2gcZRgH4ARbqFiR2qaxye7sboKGoqAQRSuIBxXxoNRqsVLoTQuKB0URq1UUiicvRcVDUUS8eVQR9FDUg9qDNwVRsCo9Kkp7EaS+r52t04+k2Ur+bXweeNn53vlmJnv7sV9mZmQEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFlm32/2qqqqfOp3OrhzH54Gob6K/s5wLAMAKiGD2YAS0ryO0fVS3RmP7+XMmAQCwciKsPdVqtS6uA1uGtdkMceU8AABWSLvdvic/I6TtifDWy3GGtnIeAAArpNfrXZufk5OTmyOoPRbB7ZmJiYkt5TwAAFZAGcw6nc4vEdj2N3sAAKyQXPqMgPZZM6DF+JDlUAAAAAAAAAAAAAAAYGR8fPySqqq+7HQ6f0WdjvozH6AbdXc5FwCA5XVR/Uqq3xtBLd8n2g9up2P/reVBAAAsg9nZ2fV1KDsWoaxb7q+Nxv7f6gB3X7lztYmwuTu+yoayDwAwlCLYPBAB57ter9cp9zVFULu5Dm0nyn1LLa65Pf7OG8t+U8zZFnUw6vWoV6I1Ws4BABg6U1NTl0W4+aLdbt9S7pvDaL6mKn+Ny+1yZ/R/yGXU81V5zKDi3K/G8VeX/ab837v4HtdHTcT2W/nLYTkHAGDoRBB6og5gg8ql0XcjFN3QbPZ6vfHp6emt/XHMORb1Y6vVmszx2NjYxhi/8+8Rg8vgNUBYuz1/YcvtvGaMX4jxB5ZFAYChF6HmyAUGtgxHT1fFXaMZmJrjOOcfUe/F5rp6/6YYH27OGVQce8fIHL/oNcW5D2QozO18xVb+fdE73g9xAABDq9vtvh2h5lTZP595Atvu5jhDYM7rjycnJzfH+OHmnEG0Wq2L47g3yn4pv0c/sMX8l+vAdqryHlQAYNgtVmBrqn9NO7scOoiYe3l15nlvzf95+znqZNSJfi/Oe7A8tl5uPZqfGdBi+yWBDQBYM5YosGVoOrsc2hTX2zOywPJmX5xjV9SzZX8O6/J6dXB7pLEkKrABAMNvKQJbnG9vzin7qbqAZdE4z5GJiYl22Z9Lfo+YuyWOea2+6UBgAwDWhsUObNWZ5dBj5XJovvIqrvViPkak2e+r7wS9Ler+uvZF7W+M/6nuPM9iq87cJfpkfD6U49g+NN+vfAAAQ2UJAlsuh+b5zglK0bs3rnVdfO5o9udTL50OrF4O/T6vn+M4/tNcGi3nAQAMncUKbFNTU1fFeX7t1O8djToZ9W0/QLVarSvjWjfFeFPzuLnkHaVx7JtlfyFxzPG4xuNRO2P7OQ/OBQDWhMUKbAup/7/s86hH81Ed5f6mDGsZ2sr+QsplWACANWG5Alv9q9kng7wCK+btLXsAAP9byxXYAAD4jwQ2AIBVTmADAFjl8u7OvLOy7J9PzL8in6tW9gEAAAAAAAAAAAAAgDUmX8g+30vZS9PT01urqtpX9gEAWCXycR6dTudo2QcAYGmM1i9K/7DcMR+BDQBgGUXw2h51V9SBHEd4uzPD2Bz1fq/Xm8k5AhsAwDKL8LUj6uPcjsC2Iba3lRVhbXx2dnZ9zhHYAACWWYSvw/mqqXa7fc3Y2NjGMqwJbAAAK290Zmbm0rIJAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAXIi/AdiSHHS7fAJWAAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAaCAYAAABVX2cEAAABS0lEQVR4XmNgGAUUAXl5eUcgfg3E/0FYTk5uh4yMDCdMXkVFhU9BQWEXTB6K14mLi3Mjm4MMGIEKZgHxLyD+CcSW6AqAYkFAvAbZIqwA6BpBoO0LgXQ+1OYpQGFGZDVAsSIgjkYWwwoUFRX1gQb1AxVLAvF1IH4CxIpISliA/NkgdUhi2AHIRqDL0kFsIN0AdV0OTF5KSkoE6nJBhC4cAKixD6jQGMSWlZXVAfLfA/EJJSUlfpAYUM4GyJ+MqgsLgIUXyHaoEMhLy4H4H1DcAyQAcjVJ4cWAFOAgQ0CGgQwFxR5Z4QUDIO+BvAn1rhOx4QVKX5OB4WSKLgHUHCMPiYhrQMM60eUxAJbwggOgt8TlIckEZCDh8AJ5AYjXAQ3jQpcDAWgyeQvEmuhycAB0kQtQwReorSAMykLe6OpAyQSUV4kJr1EwCoYMAABopFha15uM6gAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAAaCAYAAAC6nQw6AAABXElEQVR4Xu2UzSsFURjG54aFkMSYmu+PmB1qUhQpWdhaKGWvZGVlS/4D2VEWVmR/FyztlK2Vkj/AQqwUfqfOmY5T484sWN2nnmbmed/3mXeemXstq4taiKJoGT6GYfhckyumh0ALk+M4ji9gLK6FiHYKP5FWZV8P50toT0EQzJbTCkmSODRcZlk2rjTuOMLAnRjyfd9Tum3bg2jnnuf5Sish1sRoV9cwn2bgFV5x2at0eYOjPM+HtPayuJ6m6aSu0bwJv6jt6TqbjKJtWfLxO0Lm88HQglmrjap8GgOjApN3Mx8dZDhFrhvWb49YlY8OTLZlVpVo0XD25/k4jjPANgfUT3jTw2a9RKd80NcwmuHYhvM/iuIbQryHLyIbjW/wQZirXracwGgO/Vpsr/s0Bkb7gqbeCK7rjrHNrdxqhw37zZ5aED8RDG7gIf8Ai2a9EYqi6BNvz9S7+Ed8A4W8XCl+8lIfAAAAAElFTkSuQmCC>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAcAAAAaCAYAAAB7GkaWAAAAxklEQVR4XmNgGGAgLi7OraCgUKikpKSGLscgLy9fBMT/gQrS0eUYpKSkRIASDsbGxqzocjgBs5ycnDEQ24DYcFGQEUDBCUC7aoH4NBD3wiWBEq5AgRoVFRU+IH0AaOdKuG4gJ1NRUVEfKGEJxN+A/Ai4ThgACjYAJZ8AsSKKBMgLQMGrQDwFyGVEkQTq8gBK/ALa7wK0Qh1kClwSKDgD5FJpaWlhUCiBHAmXlJWV9QPZBxTcANRVwIButKioKA9QQgBFcIQAADeCJYgIvG3hAAAAAElFTkSuQmCC>