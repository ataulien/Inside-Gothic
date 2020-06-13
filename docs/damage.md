# Damage Types

This chapter sheds light onto the different kinds of damage a character can experience and how the damage affects it.

Gothic defines the following damage types:

| Type    | Used by                                           | Additional info                      |
| ------- | ------------------------------------------------- | ------------------------------------ |
| Barrier | Barrier, duh                                      | Lighting uses Barrier + Magic damage |
| Blunt   | Blunt Weapons, Golem, default for Spells          |                                      |
| Edge    | Sharp weapons, monsters with sharp teeth or claws |                                      |
| Fire    | Firewaran, Firegolem, Spells                      |                                      |
| Fly     | Golem, Troll, Stormfist spell                     | Knocks the target into the air.      |
| Magic   | Most spells                                       |                                      |
| Point   | Bows, Crossbows                                   |                                      |
| Fall    | Falldamage                                        |                                      |

!!! note

    Damage-types can be combined!

## Damage Descriptor

With each Damage to be applied, a Damage-Descriptor is filled. It can contains the following (often optional) properties:

**Inflictor and target:**

- Attacking Npc
- Attacking Object
- Object to Damage

**Knockback:**

- Direction the hit object should fly into (Damage type `Fly`)

**Visual:**

- Particle-Effect to trigger on Hit

**Damage amount:**

- Damages for each used Damage-Type
- Total damage to apply
- Damage multiplier

**Damage Origin:**

- Item the damage originates from (Weapon)
- Spell information (Spell type, level, category)
- Weapon-Kind (Fist, Melee, Ranged, Magic, Special)
- Hit location

**Damage over Time:**

- How long the damage should stay active
- How often it should be applied (Interval)
- Damage per interval

**Lethality:**

- Whether the damage should kill the hit Character

## Applying damage

Damage is usually applied as an [Action from the Action-Queue](ActionQueue.md). Damage-over-time is usually applied as an Overlay, so other Actions can be executed simultaneously.

There is a condition checking whether the Npc is not Dead, Unconscious or blocked by magic. Only if neither of those apply damage can be applied.

The following spell-effects count as "restraining the Character" and must **not** be active:

- Icecube
- Whirlwind
- Green Tentacles
- Icewave
- Pyrokinesis
- Suck Energy

!!! warning

    While these spells are checked, the check does not seem to work in the game, as you can in fact damage enemies while they are frozen!

    (I probably just haven't figured that out completely yet)

Additionally to the field in the Damage Descriptor saying whether the inflicted damage should _not_ be lethal, there are further conditions about that:

For Damage to be non-lethal, the attacker

- must use a blunt weapon if it is **non-human**
- must use a blunt or edged weapon if it is **human**

Additionally, the victim itself must be human and cannot be in water.

!!! note

    An unconscious character's hitpoints are set to 1.

## Dead or knocked out?

In Gothic, Monsters will kill Humans immediately and vice-versa. However, Humans will _always_ knock out other Humans first, leaving them on the ground with 1 hp!

Whether the attacker finishes the victim is up to the scripts. Right after the attack is over, the following [script-code](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/AI/ZS_Human/ZS_Attack.d#L211) is executed (shortened):

```c
if ((Npc_GetPermAttitude(self, other) == ATT_HOSTILE)
||	((C_GetAttackReason(self) == AIV_AR_INTRUDER) && Npc_HasNews(self, NEWS_DEFEAT,self, other)))
{
    AI_FinishingMove(self, other);
}
else
{
    B_Say (self, other, "$NEVERTRYTHATAGAIN" );
};
```

This snippet checks whether the victim is

- considered hostile
- or is an intruder (ie. walking towards Guards) and has been knocked out before

If one of those conditions is true, the victim will by killed once it is laying on the ground unconscious.
