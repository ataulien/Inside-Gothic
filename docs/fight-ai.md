# Fight AI

Both monsters and humans use the same code for their Fight AI, which behaves differently depending on the weapons the character has access to.

!!! note

    Monsters are always in *Fist-Mode*, even when not attacking.

    An attacking Scavenger is the same as a fistpunch for the Fight-AI.

## Situations

Which move the character should do next is depending on the situation it finds itself in.
Here is a complete list of all situations:

**Priority 1: Reacting to enemy actions**

- Enemy is about to hit me!
- Enemy is doing a storm-attack!
- I got the enemy in focus and they're turning towards me to hit me!

**Priority 2: Enemy is in melee range**

- I'm in a weapon-combo
- I'm running towards my enemy
- I'm strafing
- I'm not doing anything, but got the enemy in focus
- Enemy is close, but not in focus!

**Priority 3: Enemy is within walking distance (melee-range \* 3)**

- I'm in a weapon-combo, but the enemy is just out of reach!
- I'm running towards my enemy, but he's still just out of reach!
- I'm strafing and the enemy is just out of reach
- Not doing anything while the enemy is just out of reach.
- Enemy just out of reach, but not focused!

**Priority 4: Enemy is in far-fighting-range (Bows, Crossbows, Magic)**

- I have the enemy in focus!
- Enemy is not focused, but within far-fighting-range!

All those situations are checked from top to bottom, in the order they are given here. Depending on the first match, the next move is determined.

!!! info

    Determination of the next move for a situation can also result in trying the next situation!

## Possible fight moves

The following fight moves can be done by the characters:

**Movement:**

- Run
- Run back
- Jump back
- Turn
- Strafe
- Turn to hit
- Stand up

**Attacks:**

- Attack [When walking: Stormattack] otherwise [Left or Right]
- Side attack [Left --> Right]
- Front attack [Left --> Forward] or [Right --> Forward]
- Triple attack [Forward --> Right --> Left] or [Left --> Right --> Forward]
- Whirl attack [Left --> Right --> Left --> Right]
- Master attack [Left --> Right --> 4x Forward]
- Parade
- Storm-Attack

**Attack Status:**

- Prehit
- Combozone
- Posthit

**Character status:**

- On ground
- Stumble
- Wait (short, 200ms)
- Wait (long, 400ms)

## Character tactics

Now with all situations and fight moves defined, we can look into how different fight-tactics are realized. Different tactics are used depending on the type of character and skill-level. The tactics are defined in the [script files](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/tree/master/gothic/_work/data/Scripts/content/AI/FAI).

For each tactic, there is a set of script instances, one for each [situation](#situations).

Here is an excerpt of the [Human-Master tactic](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/AI/FAI/FAI_Human_Master.d#L25):

```c
INSTANCE FA_ENEMY_STORMPREHIT_4 (C_FightAI)
{
	move[0] = MOVE_STRAFE;
};

INSTANCE FA_MY_W_COMBO_4 (C_FightAI)
{
	move[0] = MOVE_WHIRLATTACK;
};

INSTANCE FA_MY_W_RUNTO_4 (C_FightAI)
{
	move[0] = MOVE_RUN;
};
```

Notice the `4` in the instances name. This number is the tactics index. The instance name itself encodes the situation to which it should apply.
Each of these instances can hold up to 6 moves. One of them is randomly chosen once the character is in that situation.

!!! example

    Here is what a skeleton would do, [if it has the enemy in focus](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/AI/FAI/FAI_Skeleton.d#L59) but is not doing anything otherwise:

    ```c
    INSTANCE FA_MY_W_FOCUS_17 (C_FightAI)
    {
        move[0] = MOVE_WAIT;
        move[1] = MOVE_STRAFE;
        move[2] = MOVE_WAIT;
        move[3] = MOVE_WAIT;
        move[4] = MOVE_ATTACK;
        move[5] = MOVE_ATTACK;
    };
    ```

    With a chance of...

    - 1/2 it will wait for 200 Milliseconds
    - 1/3 it will attack
    - 1/6 it will strafe in a random direction

    The order in which the moves are specified does not matter. The engine simply chooses one random move.
