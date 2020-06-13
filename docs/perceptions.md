# Perceptions

Whenever a Character notices (_assesses_) something in the world around it, a _Perception_ is triggered. For each perception, a script-state can be registered to be started once that perception fires.

Depending on the script-state entered upon assessing something, perceptions may be set up different, ie. Npcs reactions differ depending on their situations.

Perceptions can be manually triggered from script-code (marked :page_facing_up:) or are sent from the engine on certain actions (marked :gear:).

## Active Perceptions

Here is a list of all perceptions checked by each character individually every frame:

- **Assess Player:**  
  :gear: Triggered when the Player is nearby.

- **Assess Enemy:**  
  :gear: Triggered when a hostile Character is nearby.

- **Assess Fighter:**  
  :gear: Triggered when a nearby Character has their weapon out.  
  :page_facing_up: Sent when someone transformed into a Bloodfly. (?)

- **Assess Body:**  
  :gear: Triggered when a nearby Character is dead or unconscious.

- **Assess Item:**  
  :gear: Triggered when an item is nearby.

!!! note

    If multiple nearby Characters apply, the perception is only triggered for the closest one.

## Passive Perceptions

Passive perceptions are usually broadcast to all nearby applicable Characters within a certain range.

!!! note

    Perception ranges can be adjusted separately from the Daedalus-scripts.

**Fighting:**

- **Assess Murder:**  
  :gear: Broadcast to nearby NPCs when someones/somethings Death was caused by another Character (Human or Monster).

- **Assess Defeat:**  
  :gear: Broadcast when a Character drops unconscious.

- **Assess Damage:**  
  :gear: Sent to the damaged NPC after receiving damage and when an attack was parried (reports 0 damage).

- **Assess Others damage:**  
  :gear: Always broadcast to nearby characters when an `Assess Damage` happened.
-
- **Assess Threat:**  
  :gear: Sent to the NPC targeted with a ranged weapon.

- **Assess Draw Weapon:**  
  :gear: Broadcast when the player drew their weapon (ONLY the player!).

- **Assess Remove weapon:**  
  :gear: Broadcast when someone un-drew their weapon.

- **Assess Warn:**  
  :page_facing_up: Broadcast if someone saw pickpocketing.  
  :page_facing_up: Broadcast if someone (weaker NPCs) is calling for guards (ie. if the player entered a forbidden room).  
  :page_facing_up: Broadcast if someone witnessed stealing an item  
  :page_facing_up: Broadcast if someone witnessed a sneaking Character (After "Observe Suspect")  
  :page_facing_up: Broadcast if a character is asking their comrades for help  
  :page_facing_up: Broadcast while monsters are warning the player (or any character) before they attack  
  :page_facing_up: Broadcast if an Orc has assessed damage  
  :page_facing_up: Broadcast if an Orc witnessed an intruder (After "Observe Intruder")

**Trickery:**

- **Assess Enter Room:**  
  :gear: Broadcast when the Character has changed rooms (went outside --> inside, inside --> outside or changed rooms within the same building)  
  :page_facing_up: Broadcast after sleeping in a bed, so that the NPCs will notice the player being in the room again instead of letting the player steal everything.

- **Observe Intruder:**  
  :gear: Broadcast when the Player stopped moving and is not sneaking. (?)

- **Catch Thief:**  
  :gear: Broadcast after pickpocketing when the victim is aware of the action.

- **Assess Theft:**  
  :gear: Broadcast when an item was picked up. Script figures out if taking the item really was theft.  
  :gear: Broadcast to nearby Characters during pickpocketing. (There better be nobody nearby...)

- **Assess Fake Guild:**  
  :gear: Broadcast to nearby characters when someone transformed into another creature, making
  them remember that you tried to fake your guild. (Better not have anyone else
  nearby when transforming!)

- **Observe Suspect:**  
  :gear: Broadcast when a Character is sneaking. Usually results in nearby NPCs asking "Why are you sneaking?".

- **Assess Surprise:**  
  :gear: Broadcast when someone transformed back from being a monster.

- **Assess Use Mob:**  
  :gear: Broadcast when a Character starts using an interactive object. (ie. fiddling with a locked chest)

**Sound:**

- **Assess Quiet Sound:**  
  :gear: Broadcast every time a footstep-sound is played. Also broadcast when two (physics) objects collided (ie. arrows hitting the ground).

- **Assess Fight Sound:**  
  :gear: Broadcast when a Character got hit during a fight.  
  :page_facing_up: Broadcast when someone got hit by magic (Assess Magic).

**Talking:**

- **Assess Call:**  
  :gear: Sent to the NPC the player (or an other NPC) wants to talk to, if it is _not_  
  within talking range. Usually plays a "Come over here" animation and waits  
  until the player comes closer.

- **Assess Talk:**  
  :gear: Sent to the NPC the player (or an other NPC) wants to talk to, if it is within  
  talking range, ie. after pressing `CTRL+Up` in front of an NPC. Usually begins a dialogue between the player and the NPC.

- **Npc Command:**  
  :page_facing_up: Used for finding a smalltalk parter.  
  :page_facing_up: Sent when an NPC wants their item back ("Give it back to me!", not sure whether that is even used, actually)

**Items:**

- **Assess Given Item:**  
  :gear: (Seems to be unused) Sent to the receiving NPC during an item-trade (ie. when shopping)

**World:**

- **Move Mob:**  
  :gear: Broadcast when a door is blocking the path of an AI controlled character.

- **Move Npc:**  
  :gear: Broadcast when an other Character is blocking the path of an AI controlled character.

**Magic:**

- **Assess Magic:**  
  :gear: Sent to NPCs hit by magic effects, has access to spell type and category.

- **Assess Stopmagic:**  
  :gear: Sent to NPCs which got hit by magic effects (`Assess Magic`) when the magic effect stopped (tied to the actual particle effect).

- **Assess Caster:**  
  :gear: Broadcast when someone starts investing mana in a magic spell.

## Perception Broadcast Ranges

Broadcast ranges can be set up for each perception individually. Therefore, only their default values are listed here:

|           Perception | Range (m) |
| -------------------: | :-------- |
|        Assess Murder | 30        |
|        Assess Defeat | 30        |
|        Assess Damage | 3         |
| Assess Others damage | 10        |
|        Assess Threat | 30        |
|   Assess Draw Weapon | 6         |
| Assess Remove weapon | 20        |
|          Assess Warn | 30        |
|    Assess Enter Room | 15        |
|     Observe Intruder | 0.5       |
|          Catch Thief | 1.5       |
|         Assess Theft | 8         |
|    Assess Fake Guild | 5         |
|      Observe Suspect | 5         |
|      Assess Surprise | 5         |
|       Assess Use Mob | 15        |
|   Assess Quiet Sound | 10        |
|   Assess Fight Sound | 20        |
|          Assess Call | 10        |
|          Assess Talk | 5         |
|          Npc Command | 5         |
|    Assess Given Item | 5         |
|             Move Mob | 5         |
|             Move Npc | 5         |
|         Assess Magic | 30        |
|     Assess Stopmagic | 30        |
|        Assess Caster | 20        |

Active perception range is set for each Character via a script property (`senses_range`).
