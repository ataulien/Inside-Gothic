# Spreading the News

Certain actions the Player or AI does is newsworthy. This includes witnessing

- a murder
- someones defeat
- a murder during a fight
- theft (pickpocketing or stealing from someones house)
- someone running away from a fight (out of reach or chasing for too long)
- using a forbidden interactive object (ie. when it belongs to someone)

Depending on whether other NPCs have seen who the evil-doer is, the news
includes the responsible character. If nobody saw who did it, the news will
include "We don't know who did it" instead. In total, a news-entry can store

- Victim
- Offender
- Witness
- What happened (Murder, theft, ...)

!!! info

    There are some unused types of news [in the scripts](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/AI/B_Human/B_AssessAndMemorize.d#L9):

     * Nerve
     * Interfere
     * HasDefeated

## Spreading

When something was witnessed by someone, a News-entry is created for that witness.
After some time, that news may spread inside the guild or to characters the
witness is friends with.

Depending on the news, the script function [C_CanNewsBeSpread()](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/AI/B_Human/C_CanNewsBeSpread.d#L1)
determines whether the news should be spread and to who.

!!! warning

    As you can see in the actual script sourcecode, the complete function contains only code which is commented out. Therefore we can deduce that the whole system is not actually used in the game.
