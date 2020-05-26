# Action-Queue

While in Gothic, every Vob can have an Action-Queue, they are most used by Npcs.
Actually, the Action-Queue is _the inner core of Npc behavior_!

The Action-Queue itself is best described as a First-In-First-Out buffer with
extra steps. It holds actions of the following categories:

- Damage
- Weapon
- Movement (Go somewhere)
- Attack (Single attacks)
- UseItem
- Script State
- Manipulate (Items/Mobs)
- Conversation
- Magic

> For a full list of categories and sub-types, basically everything an Npc can
> do in Gothic, see [this document](EventMessages.md).

On every frame, the action at the front of the queue is executed. The action
itself can decide whether it is done after it was executed, or if it needs to be
executed again next frame:

- If the action says it is **done**, it will be popped from the queue and the
  **next action will be executed** next frame.
- If the action is **_not_ done** yet, it will stay at the front of the queue
  and **will be executed again** next frame.

## Example

Let's assume the following sequence of actions has been pushed into an Npcs Action-Queue:

| **Index**  |       0       |     1     |        2        |            3            |         4          |
| :--------: | :-----------: | :-------: | :-------------: | :---------------------: | :----------------: |
| **Action** | Goto Location | Take Item | Use Item (Beer) | Goto Character (Player) | Give Item (5x Ore) |

The Action at index 0 (_Goto Location_) is at the front of the queue and thus
will be executed for as long as the Npc is not at the target location.

On every frame, when the _Goto Location_ action is executed, it will check
whether the Npc arrived at its destination. Lets say, it did. Then, the action
will be popped off the Queue and the next one takes its place. The Queue now
looks like this:

| **Index**  |     0     |        1        |            2            |         3          |
| :--------: | :-------: | :-------------: | :---------------------: | :----------------: |
| **Action** | Take Item | Use Item (Beer) | Goto Character (Player) | Give Item (5x Ore) |

The next Action is now at the front of the queue and will be executed. For _Take
Item_, it will block the queue until all animations have been finished and the
item is in the inventory (if the Npc could pick it up, that is). After the item
was picked up, the Action signals that it is done. The queue now looks like
this:

| **Index**  |        0        |            1            |         2          |
| :--------: | :-------------: | :---------------------: | :----------------: |
| **Action** | Use Item (Beer) | Goto Character (Player) | Give Item (5x Ore) |

And so on, you get the Idea.

## Filling the Queue via Scripting

The Action-Queue is the most important system to let the games scripts work in
the way they do. Many of the Npc related script external functions actually just
push an action into the queue and continue with executing the script.

Most commonly used in dialogues, the `AI_Output` external lets a character say something. In the original scripts, a dialogue [can look like this](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/Story/MISSIONS/DIA_Pc_Thief.d#L28):

```c
FUNC void  PC_Thief_WHEEL_Info()
{
  AI_Output(self, hero,"PC_Thief_WHEEL_Info_11_01"); //Die Winde scheint zu klemmen.
  AI_Output(self, hero,"PC_Thief_WHEEL_Info_11_02"); //Lass mich mal, vielleicht kann ich da was machen!

  AI_StopProcessInfos(self); // Cancels dialogue

  AI_GotoWP(self,"LOCATION_12_14_WHEEL");
  AI_AlignToWP(self);

  AI_PlayAni(self,"T_PLUNDER");
};
```

In this dialogue option, Diego tells the Hero "Let me try" and goes to the
broken wind to turn it. All those `AI_`-functions push an action into Diegos
Action-Queue and are executed sequentially, one after another.

## Overlay Events

Usually, only the event at the front of the queue is active and passed to
the target object. However, events marked as "Overlay" will act a little
bit different.

If such an overlay-event is at the front of the queue, it will be passed
to the target object every update cycle. Then, the next event in the queue
is looked at. If that is also an overlay-event, it is passed to the object as
well.

This continues until the first event is encountered which is not an
overlay. That will be the last event passed to the object.
