# **Quests Types**

---

## **Quest Types and Progress Triggers**

**Cobblemon BattleHUB** ships with a wide range of battle objectives. Each quest's behavior is determined by its `type`.

Below is the detailed table of all natively supported quest types, how progress is calculated, and how to use the `typeFilter` parameter in each scenario.

---

## **Quest Trigger and Behavior Table (QuestType)**

| Technical Key (type) | Filter (typeFilter) | Behavior and Progress |
| :---- | :---- | :---- |
| **`PLAY_MATCHES`** | *Not used* | Increases ![][image1] progress for every completed match (Ranked or Casual). |
| **`WIN_MATCHES`** | *Not used* | Increases ![][image1] progress whenever the player wins a match in any queue. |
| **`WIN_RANKED`** | Format (e.g. singles, doubles) | Increases ![][image1] progress if the player wins a **Competitive** match that matches the filter format. If the filter is `null`, any ranked format is accepted. |
| **`PLAY_RANKED`** | Format (e.g. singles, doubles) | Increases ![][image1] progress if the player completes a **Competitive** match in the specified format. |
| **`PLAY_CASUAL`** | *Not used* | Increases ![][image1] progress when completing matches in the server’s Casual queues. |
| **`WIN_STREAK`** | *Not used* | Tracks consecutive wins. If the player wins, they gain ![][image1] progress. **Warning:** If the player loses a match, this quest’s progress resets to ![][image2]! |
| **`USE_TYPE`** | Elemental type (e.g. fire, ghost) | Increases ![][image1] progress if the player completes a battle using at least **one** Pokémon of the specified type in their main party. |
| **`WIN_WITH_ALIVE`** | *Not used* | Increases ![][image1] progress whenever the player wins a match (focused on survival strategies). |
| **`WIN_FAST`** | *Not used* | Increases ![][image1] progress if the player wins the match in a total of ![][image3] turns or less (`targetAmount` defines the value). |
| **`PLAY_LONG`** | *Not used* | Increases ![][image1] progress if the match lasts ![][image3] turns or more (or exceeds the overall 20-turn limit). |
| **`WIN_BY_FORFEIT`** | *Not used* | Increases ![][image1] progress if the opponent forfeits the match (surrender/Alt+F4), awarding victory to the player. |
| **`PLAY_NO_FORFEIT`** | *Not used* | Increases ![][image1] progress by completing a full match without either side forfeiting. |
| **`PLAY_MONOTYPE`** | *Not used* | Increases ![][image1] progress if the player enters a battle with a team that follows Monotype rules (all Pokémon share a common type). |
| **`KNOCKOUT_TOTAL`** | *Not used* | Balances knockout progress. Grants ![][image4] points if the player wins the match, or ![][image1] point if they lose, reflecting average battle aggression. |
| **`MASTER_MONOTYPE`** | Elemental type (e.g. water, steel) | **Direct Profile Sync:** This type tracks total Monotype wins in the database. The mod automatically syncs the player’s progress with the total Monotype victories recorded in their global stats profile (`StatsManager`). |

---

## **How the Format Filter Works**

The `typeFilter` parameter uses a partial validation system (`formatMatches`). It checks whether the technical Ladder ID played contains the word specified in the filter.

### **Practical Example:**

If you create a quest with:

* `type: "WIN_RANKED"`  
* `typeFilter: "doubles"`

The system will validate progress if the player wins on a ladder such as `doubles_ranked` or `doubles_50_ranked`, because both contain the keyword `doubles`. If the filter is configured as `null` or `""`, the mod ignores validation and accepts any ranked format.

---

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABoAAAAZCAYAAAAv3j5gAAAAzklEQVR4XmNgGAUjCsjLyxvKysq6oYtTBSgoKJjLycmVAS05DcT/gexydDV4gaKiojhQ4yxxcXFudDlkALXIF6jWC4i/kmwRUJMk0JCFoqKiPOhy2ADQAuNRi8CAJhYBDRQAGYyMgYlBH0ivlpGRUUGXw2Y5QYtAqQqooBqIZ6HhpUB8H6hxPha5ZHRzCFqEC4BcTvWgwwYGvUVAXIUuhxcQaxFQTQZQ7TMg/o+E3wHxYWVlZTF09RiAWIsoBiALgMERCmSyoMuNglEwNAAA0OVRFOvBHZ8AAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAkAAAAWCAYAAAASEbZeAAAA30lEQVR4XmNgGMRARkaGU15ePgqIZwFxl6KiojqKAiUlJX6gxG4gbhYVFeVRUFAwALKvAXEwXJGcnFw5UOA0kBaEiQH50UB8HWiiOEiBIEgBUPdCuC4gkJWVNQWKfwHSfiAdmkD8Fl0RULMxUPwrELfCObgUgcWBHF8g5z9eRUCGJ0FFxFqnBOQ8x6UIiKsYQIEHZBwA4q1AhRxIilyAYr9ANEwgBogfAQUVoWoYgexmID4Big2wiLGxMStQ0XSg4H6gaQFQBVdB0QPVBAeMQF1qQMUhwKiwA2lEVzCYAAC0J0evUZaWqQAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABEAAAAWCAYAAAAmaHdCAAABMklEQVR4XmNgGAV4gby8vCMQvwbi/yAsJye3Q0ZGhhMmr6KiwqegoLALJg/F65DNgAFGoMQsIP4FxD+B2BJdAVAsCIjXIFuAAoC2CwJtWwik86E2TQEKMyKrAYoVAXE0shgKUFRU1Aca0A9UJAnE14H4CRArIilhAfJng9QhiaECkA1Al6SD2EC6AeqaHJi8lJSUCNSlgghdaACooQ+owBjElpWV1QHy3wPxCSUlJX6QGFDOBsifjKoLCcDCA2QbVAjk9OVA/A8o7gESALkSb3ggewUGQC4AuQTqIidCXgFF7WSgF0zRJYCaYqBhcw1oSCe6PBxg8QocAGNCXB4SUyCD8HrFCYjXAQ3hQpcDAWhMvQViTXQ5kAtcgBJfoLaAMCiVeqOrA8UUKBvgC49RMBgBAJmsUaA7kqF0AAAAAElFTkSuQmCC>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABcAAAAWCAYAAAArdgcFAAABgUlEQVR4Xu2TP0vDUBTFU1QQVBQkBtukadNAR5V8AXEQFxfRqaObq5siCPopRJAOTg5OOouOQp2KiyKC6KabuNT6u22SpheR+GcReuDQ++4997x330sNo4d/Cdd1J+EW3IPr2WzW0ZofIZ/PL2J4WiwWp3zfN4l34Btc0toYiC05iWVZQ7oWoVAoDKI5gS9wRnJs5hE/wbrWx5BRaa6apjmsaxFC82PYIJ6VXC6Xs1k/wFsl7yCNuUA2wHCcMBOuF+h9h/tK2kFa8yTk1PSc03spsa7H+I55qVSaQH8RXkdNHtcIJ5FRxsQsSRHwe2Tbtq9rX21IfQ6+4rltyNfAYtNtf6NJHsI7Xv/gk9qqNo1QLpdH3PYUDV2L4aa4Fs/zRuWEaCtGdA1G6yaq5JoJaTfSmId/oCa8F73kRE989hfmAbpnuBsEwYDkwjd6hNdaHyONOcigW4N1uAEr8Are0DutxTFSmrcgd+84zjyTLMs0pPq0pgtiinCFsF/XevgVPgBM+mcIEeMAbgAAAABJRU5ErkJggg==>