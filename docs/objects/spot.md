# Freepoint (Spot)

Spots (also known as Freepoints) are invisible objects scattered through
the world. They are only needed for their location and name and could be
compared to a [Plain Axes
Empty](https://docs.blender.org/manual/en/latest/modeling/empties.html)
from Blender.

Often, Spots are scattered around fireplaces or monster locations. A
Spot can then be used by characters or monsters for certain actions. For
example, they tell the Npcs where they can sit. If a spot is occupied,
the Npc will simply choose the next available spot. If that is also
occupied it will usually just stand somewhere, trying to do smalltalk.

For Monsters, Spots define where they can go when they want to go for a
little walk around they spawning location (Roaming).

All of that is realized by giving each Spot a special name, ie. `FP_SIT`
or `FP_ROAM`. There can be many Spots with the same name, but the game
just checks whether the tag is _somewhere_ in the name. The process goes
like this:

1.  _Is there any place where I can sit down at?_
2.  \*Searches for nearby unoccupied `FP_SIT` spots\*
3.  _Ah, there by the campfire is a nice spot! I'll occupy that for a
    moment!_
