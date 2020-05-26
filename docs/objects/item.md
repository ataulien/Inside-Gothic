# Item

Everything you can pick up from the ground is using the `Item` class. They have
the following important properties:

- Display name
- Item category (Potion, weapon, ...)
- Value in Gold/Ore
- Description Text

Then, they _all_ have properties regarding consumables, armor, weapons, and so
on, which are just disabled by default. That could be:

- Script function to run on use, equip and unequip
- New Visual for the character who equipped this (for armors)
- Magic cycle and script functions
- Guild and fake-Guild
- Spell range
- For a full list, see [this](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/_Intern/CLASSES.D#L79).

Depending on the item category, some of these are used while the others are
ignored. I imagine this could have been made nicer be creating more classes, and
not shoving everything into one base class...

## Script Dependency during World Load

Items can be spawned via script, but usually are loaded straight from the
world-file (`.zen`). However, the visual of the item is always defined in the
script instance!

For example, let's take a look at the [Health Potion instance
definition](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/Items/Potions.d#L164)
(see `Mesh to use` comment):

```c
INSTANCE ItFo_Potion_Health_01(C_Item)
{
  name        = NAME_Trank;

  mainflag    = ITEM_KAT_POTIONS;
  flags       = ITEM_MULTI;

  value       = Value_HpEssenz;

  visual      = "ItFo_Potion_Health_01.3ds"; // <------------ Mesh to use
  material    = MAT_GLAS;
  on_state[0] = UseHealthPotion;
  scemeName   = "POTIONFAST";

  description = "Essenz heilender Kraft ";
  TEXT[1]     = NAME_Bonus_HP; COUNT[1] = HP_Essenz;
  TEXT[5]     = NAME_Value;    COUNT[5] = Value_HpEssenz;
};
```

In the world-file itself, only the instances name is stored, such as
`ItFo_Potion_Health_01` in the example above.

This requires you to have the Daedalus-VM ready while loading the world, if you
want to place all items on that step. Without being able to run instance
constructors you're not going to know how the items should look like.

!!! note

    This is especially unfortunate, since the visual is the only missing piece at
    load time. You could delay running the script instance constructor until the
    item is picked up and write a full world importer without ever touching the
    scripts...

    If you are dealing with items from the original game only, you may precompute
    the mapping of script instances to visuals, but that would not work for mods
    with custom items.
