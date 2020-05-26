# Monster AI

As hinted in the Chapter [Daily Routine](DailyRoutine.md), Monsters do not use the same Daily Routine code Humans are using. Each Monster is actually using the [Monster Scheduler](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/AI/ZS_Monster/ZS_MM_Master.d#L526).

## Monster Scheduler

The following start and end times can be set for each type of Monster and are relevant to the scheduler:

- Sleep time
- Rest time
- Roaming time
- Eat-Ground time
- Wusel time [^1]

Each of them can be set as being _always active_, but they are prioritized in the order given above, top to bottom.

!!! example

    When _Roaming_ is set to be _always active_, but
    the time for _Sleep_ is satisfied, the Monster will go Sleeping, since that has a higher priority.

Depending on the ingame clock time, one of the matching sub-states is entered.
If none of the times are satisfied, a default state is entered.

All states will wait a random time up to 1 Second so that nearby Monsters will not start their actions at the exact same time.

[^1]: Hard to translate German word for a swarm of sth. walking around aimlessly. (To bustle around?)

### Sleep State

The following effects are applied when the state is initialized:

1. Monsters perception time is increased to 2 Seconds, so it takes slightly longer to notice things going on around it.
2. Predators will **not** prioritize this Monster in this state. (Why ever that is)
3. The Monster is told to walk (instead of run).

Then, the following actions are executed:

4. If the closest waypoint is not the one where it was spawned, the Monster will run "home" before laying down.
5. Then, the Monster will walk to the next unoccupied [Freepoint](objects/spot.md) `FP_SLEEP` if possible.
6. A "looking around, making sure everything is ok" animation is played
7. The Monster plays its _Lay down to sleep_ animation.

Then, the state is looped until the sleep time is not satisfied anymore.

### Rest State

The following effects are applied when the state is initialized:

1. Monsters perception time is increased to 2 Seconds, so it takes slightly longer to notice things going on around it.
2. Predators will prioritize this Monster in this state.
3. The Monster is told to walk (instead of run).

Then, the following actions are executed:

1. If the closest waypoint is not the one where it was spawned, the Monster will run "home" before resting.
2. Then, the Monster will walk to the next unoccupied [Freepoint](objects/spot.md) `FP_ROAM` if possible.

After initialization, on each loop update:

1.  One of three random roam (yes, roaming, not resting) animations may be started with a chance of roughly 0.5%.

!!! info

    There is a bug in the original scripts here. The code rolls the random number via `Hlp_Random(2)`, which is implemented via the standard C construct `rand() % max`.
    However, the modulo operation will return a number from `0` to `max - 1`!

    Therefore, `Hlp_Random(2)` will only return either `0` or `1`. This script code will never choose the third animation.

### Roaming State

The following effects are applied when the state is initialized:

1. The Monster is told to walk (instead of run).

Then, the following actions are executed on every state-loop update:

1. If the closest waypoint is not the one where it was spawned, the Monster will run "home" before roaming.
2. With a chance of 20%, walk to the next unoccupied [Freepoint](objects/spot.md) `FP_ROAM`.  
   If no such Freepoint exists, the Monster will walk to the closest Waypoint and then to the Waypoint _after_ that.
3. If the chance was not met, one of three random roam animations may be started with a chance of roughly 0.5%.

!!! info

    There is a bug in the original scripts here. The code rolls the random number via `Hlp_Random(2)`, which is implemented via the standard C construct `rand() % max`.
    However, the modulo operation will return a number from `0` to `max - 1`!

    Therefore, `Hlp_Random(2)` will only return either `0` or `1`. This script code will never choose the third animation.

### Eat-Ground State

The following effects are applied when the state is initialized:

1. Monsters perception time is increased to 2 Seconds, so it takes slightly longer to notice things going on around it.
2. Predators will prioritize this Monster in this state.
3. The Monster is told to walk (instead of run).

Then, the following actions are executed:

1. If the closest waypoint is not the one where it was spawned, the Monster will run "home" before eating.
2. Then, the Monster will walk to the next unoccupied [Freepoint](objects/spot.md) `FP_ROAM` if possible.
3. At last, 3 eating animations are scheduled, each playing for a random time of up to 8 seconds.

There are no actions in the loop update.

### Wusel State

The following effects are applied when the state is initialized:

1. The Monster is told to run (instead of walk).

The loop update is the same as in the [Resting](#rest-state) State: The Monster will walk to random locations.

### Default State

While in this state, the Monster will simply turn towards the Waypoint it was spawned at but stay still otherwise.

## Prey and Predator

Some Monsters can have Prey and Predator relationships between each other. In Gothic I, only two of those relationships exist:

| Prey       | Predator    | Note          |
| ---------- | ----------- | ------------- |
| Scavenger  | Snapper     |               |
| Molerat    | Wolf        |               |
| (Goblin)   | (Lurker)    | Commented out |
| (Bloodfly) | (Scavenger) | Commented out |

## Reacting to Damage

Once a Monster is being hit, it choses a different action depending on whether the attacker is a predator:

- If the Monster is being hunted by a predator, it enters the [Fleeing](#fleeing) state.
- Otherwise, it starts to defend itself by [attacking back](#attacking-state).

Monsters can also notice when any other Npc or Monster is damaged:

- If the attacked Monster is of the same species and is being attacked by a predator, the Monster will also [flee](#fleeing-state).
- If a friend is being attacked, and the attacker is not friends with the monster, the monster will [help its mate](#attacking-state).
- If a friend attacks something else, which is not friends with the monster, the monster will also [help its mate in the fight](#attacking-state).

If the monster is in a fight already, the rules are a bit simpler:

- If the monster is being damaged by a predator, it [flees](#fleeing-state).
- If the monster is damaged by the hero, it forgets everything else and [attacks](#attacking-state) him.

!!! note

    Attacking summoned monsters will also react to the summoner taking damage.

### Fleeing State

### Attacking State

The following effects are applied when the state is initialized:

1. The Monster is told to run (instead of walk)
2. The target is determined and locked in.
3. A warning is sent to nearby monsters (Makes the target flee or let the pack help in the attack)

During the loop update, the following actions are executed:

- If the target is dead, the attack is over and the monster will start [eating the target](#eat-ground-state). [^2]
- If the target is running away, and the time the monster is allowed to follow is passed, the attack is canceled.
- If the target is running away through water, the attack is canceled right away (depending on the monsters properties). [^3]

!!! note

    If the target is *Invincible*, it is not attacked. This is a dirty hack to ignore characters who are talking right now.

[^2]: The monster may wander off to a different freepoint or waypoint as specified in the [Eat Ground State](#eat-ground-state), which I suppose is not intended.
[^3]: Orcs HATE water!
