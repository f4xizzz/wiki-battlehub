# **Team Validation**

---

## **Team Validation**

To ensure competitive and casual matches occur fairly and without battle engine errors, **Cobblemon BattleHUB** uses a strict team analysis system.

This validator checks the player’s party at the exact moment they try to enter a queue or accept a duel invitation, immediately blocking any irregularity.

---

## **1. Structural Survival Checks**

Before analyzing format-specific rules, the system performs two basic infrastructure checks:

* **Minimum Team Size:** The validator checks whether the player has the number of Pokémon required by the Ladder (e.g., 6 for standard Singles).  
* **Health Status:** The mod scans the player’s party to ensure they have **at least one healthy Pokémon** (not fainted). If all party Pokémon are fainted, queue entry is blocked with the warning: *"All of your Pokémon are fainted. Heal your team before entering battle."*

## **2. Competitive Clauses (Ranked Rules)**

If the active Ladder is competitive (`ranked: true`) or a tournament format (`tourney_`), the validator applies the following restrictions:

### **Species Clause**

Prevents the player from using two or more Pokémon of the same species on the team.

* The system normalizes species IDs (removing special characters and converting to lowercase) to prevent workarounds using alternate forms or names.

### **Item Clause**

Ensures that each held item in the party is unique.

* If two Pokémon are holding the same item (for example, two Leftovers), the system blocks entry.  
* Empty items (air or no item) are ignored by the filter.

### **Banned Items**

The validator checks whether any held item on the team is registered in the format’s global banned-item list (such as the official VGCRules bans).

## **3. Automatic Meta Filters (Mythicals and Paradoxes)**

Unlike other mods where you must manually register hundreds of Pokémon in ban lists, BattleHUB scans Cobblemon’s code directly for the game’s native labels.

This guarantees a 100% accurate block of any creature classified as mythical or paradox by Cobblemon itself, keeping your configuration always up to date with future mod additions.

## **4. Monotype Validation Algorithm**

If the Ladder format includes the monotype suffix or rule, the validator executes a mathematical **Set Intersection** algorithm.

For the team to be valid for Monotype, the resulting intersection of all Pokémon type sets in the team must not be empty. In other words:

![][image1]

Where:

* ![][image2] is the number of Pokémon in the party.  
* ![][image3] represents the set of types of Pokémon ![][image4] (which may have 1 or 2 types).

---

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAmwAAABUCAYAAAA/I2vMAAAFCklEQVR4Xu3dT2gcZRgH4ARbqFiR2qaxye7sboKGoqAQRSuIBxXxoNRqsVLoTQuKB0URq1UUiicvRcVDUUS8eVQR9FDUg9qDNwVRsCo9Kkp7EaS+r52t04+k2Ur+bXweeNn53vlmJnv7sV9mZmQEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFlm32/2qqqqfOp3OrhzH54Gob6K/s5wLAMAKiGD2YAS0ryO0fVS3RmP7+XMmAQCwciKsPdVqtS6uA1uGtdkMceU8AABWSLvdvic/I6TtifDWy3GGtnIeAAArpNfrXZufk5OTmyOoPRbB7ZmJiYkt5TwAAFZAGcw6nc4vEdj2N3sAAKyQXPqMgPZZM6DF+JDlUAAAAAAAAAAAAAAAYGR8fPySqqq+7HQ6f0WdjvozH6AbdXc5FwCA5XVR/Uqq3xtBLd8n2g9up2P/reVBAAAsg9nZ2fV1KDsWoaxb7q+Nxv7f6gB3X7lztYmwuTu+yoayDwAwlCLYPBAB57ter9cp9zVFULu5Dm0nyn1LLa65Pf7OG8t+U8zZFnUw6vWoV6I1Ws4BABg6U1NTl0W4+aLdbt9S7pvDaL6mKn+Ny+1yZ/R/yGXU81V5zKDi3K/G8VeX/ab837v4HtdHTcT2W/nLYTkHAGDoRBB6og5gg8ql0XcjFN3QbPZ6vfHp6emt/XHMORb1Y6vVmszx2NjYxhi/8+8Rg8vgNUBYuz1/YcvtvGaMX4jxB5ZFAYChF6HmyAUGtgxHT1fFXaMZmJrjOOcfUe/F5rp6/6YYH27OGVQce8fIHL/oNcW5D2QozO18xVb+fdE73g9xAABDq9vtvh2h5lTZP595Atvu5jhDYM7rjycnJzfH+OHmnEG0Wq2L47g3yn4pv0c/sMX8l+vAdqryHlQAYNgtVmBrqn9NO7scOoiYe3l15nlvzf95+znqZNSJfi/Oe7A8tl5uPZqfGdBi+yWBDQBYM5YosGVoOrsc2hTX2zOywPJmX5xjV9SzZX8O6/J6dXB7pLEkKrABAMNvKQJbnG9vzin7qbqAZdE4z5GJiYl22Z9Lfo+YuyWOea2+6UBgAwDWhsUObNWZ5dBj5XJovvIqrvViPkak2e+r7wS9Ler+uvZF7W+M/6nuPM9iq87cJfpkfD6U49g+NN+vfAAAQ2UJAlsuh+b5zglK0bs3rnVdfO5o9udTL50OrF4O/T6vn+M4/tNcGi3nAQAMncUKbFNTU1fFeX7t1O8djToZ9W0/QLVarSvjWjfFeFPzuLnkHaVx7JtlfyFxzPG4xuNRO2P7OQ/OBQDWhMUKbAup/7/s86hH81Ed5f6mDGsZ2sr+QsplWACANWG5Alv9q9kng7wCK+btLXsAAP9byxXYAAD4jwQ2AIBVTmADAFjl8u7OvLOy7J9PzL8in6tW9gEAAAAAAAAAAAAAgDUmX8g+30vZS9PT01urqtpX9gEAWCXycR6dTudo2QcAYGmM1i9K/7DcMR+BDQBgGUXw2h51V9SBHEd4uzPD2Bz1fq/Xm8k5AhsAwDKL8LUj6uPcjsC2Iba3lRVhbXx2dnZ9zhHYAACWWYSvw/mqqXa7fc3Y2NjGMqwJbAAAK290Zmbm0rIJAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAXIi/AdiSHHS7fAJWAAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAaCAYAAABVX2cEAAABS0lEQVR4XmNgGAUUAXl5eUcgfg3E/0FYTk5uh4yMDCdMXkVFhU9BQWEXTB6K14mLi3Mjm4MMGIEKZgHxLyD+CcSW6AqAYkFAvAbZIqwA6BpBoO0LgXQ+1OYpQGFGZDVAsSIgjkYWwwoUFRX1gQb1AxVLAvF1IH4CxIpISliA/NkgdUhi2AHIRqDL0kFsIN0AdV0OTF5KSkoE6nJBhC4cAKixD6jQGMSWlZXVAfLfA/EJJSUlfpAYUM4GyJ+MqgsLgIUXyHaoEMhLy4H4H1DcAyQAcjVJ4cWAFOAgQ0CGgQwFxR5Z4QUDIO+BvAn1rhOx4QVKX5OB4WSKLgHUHCMPiYhrQMM60eUxAJbwggOgt8TlIckEZCDh8AJ5AYjXAQ3jQpcDAWgyeQvEmuhycAB0kQtQwReorSAMykLe6OpAyQSUV4kJr1EwCoYMAABopFha15uM6gAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAAaCAYAAAC6nQw6AAABXElEQVR4Xu2UzSsFURjG54aFkMSYmu+PmB1qUhQpWdhaKGWvZGVlS/4D2VEWVmR/FyztlK2Vkj/AQqwUfqfOmY5T484sWN2nnmbmed/3mXeemXstq4taiKJoGT6GYfhckyumh0ALk+M4ji9gLK6FiHYKP5FWZV8P50toT0EQzJbTCkmSODRcZlk2rjTuOMLAnRjyfd9Tum3bg2jnnuf5Sish1sRoV9cwn2bgFV5x2at0eYOjPM+HtPayuJ6m6aSu0bwJv6jt6TqbjKJtWfLxO0Lm88HQglmrjap8GgOjApN3Mx8dZDhFrhvWb49YlY8OTLZlVpVo0XD25/k4jjPANgfUT3jTw2a9RKd80NcwmuHYhvM/iuIbQryHLyIbjW/wQZirXracwGgO/Vpsr/s0Bkb7gqbeCK7rjrHNrdxqhw37zZ5aED8RDG7gIf8Ai2a9EYqi6BNvz9S7+Ed8A4W8XCl+8lIfAAAAAElFTkSuQmCC>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAcAAAAaCAYAAAB7GkaWAAAAxklEQVR4XmNgGGAgLi7OraCgUKikpKSGLscgLy9fBMT/gQrS0eUYpKSkRIASDsbGxqzocjgBs5ycnDEQ24DYcFGQEUDBCUC7aoH4NBD3wiWBEq5AgRoVFRU+IH0AaOdKuG4gJ1NRUVEfKGEJxN+A/Ai4ThgACjYAJZ8AsSKKBMgLQMGrQDwFyGVEkQTq8gBK/ALa7wK0Qh1kClwSKDgD5FJpaWlhUCiBHAmXlJWV9QPZBxTcANRVwIButKioKA9QQgBFcIQAADeCJYgIvG3hAAAAAElFTkSuQmCC>