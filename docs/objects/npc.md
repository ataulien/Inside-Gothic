# NPC

The Npc class is used for **Humans** and **Monsters**. There is no
difference, no subclass, it is all handled using some if-else-magic,
visuals and the animation statemachine. Even the Player is an NPC! That
makes no sense and that is why in REGoth, for example, I chose to name
them _Characters_.

The inner workings of NPCs are a huge topic for itself. But I'm going to
give a broad overview:

- **Visual:** Whether the Character is a Monster, a Human or an Orc is
  determined using the Visual, which is always a Skeletal-Mesh.

- **Event-Queue:** Most of the times when an NPC is told to do
  something via scripting, that action is pushed the characters
  _Event-Queue_. That queue then contains actions like _Walk to some
  location_, _Pick up that item_, _Attach the player_, which are all
  played back in order. The next action is started, when the action in
  front of the queue is completed.

  That queue can be interrupted and cleared by certain actions, such
  as catching fire.

- **AI:** While actions are usually executed by the _Event-Queue_,
  something needs to fill that queue. This can be done from the games
  scripts or by the AI, which includes handling fighting and other Npc
  tasks.

- **Inventory:** Each Character has an inventory to store items in. If
  you played Gothic, you will have noticed that merchants have two
  different inventories: One for selling stuff and one if you knock
  them over and want to loot them.

  Both of these are handled using a single inventory. If a merchant
  gets knocked out, their shop-inventory gets replaced by their
  loot-inventory by scripts. There is actually a bug in which that
  swap was forgotten on the Seekers if they died from an Iceblock
  spell, which allowed you to loot their spells and armor.

- **Information-Knowledge:** Every Character has a Database of which
  dialog options the player has told to them. From the script, that
  database can be queried to see whether an Npc has knowledge about
  something.

- **Daily Routine:** If the Event-Queue is empty, the characters daily
  routine is started. The daily routine specifies where the Npc should
  be on certain times over the day.

  An example would be:

  - From _06:00_ to _12:00_: Sit at Campfire near the entrance of
    the Old Camp.
  - From _12:00_ to _22:00_: Make swords at the forge.
  - From _22:00_ to _06:00_: Sleep in the bed closest to your
    starting location.

  The daily routine makes heavy use of Mobs and Spots to get the Npc
  through the day. Routines usually spawn sub-routines, also known as
  "States", which define the order in which the Npc should use the
  tools in the forge to make a weapon.

  Monsters do not have a Daily Routine. They are controlled by a
  simpler scheduling mechanism: Sleep, Eat, Roam, Repeat.
  Additionally, they can flee from predators (e.g. Scavengers flee
  from Snappers if attacked).
