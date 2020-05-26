# Dialogue-System

![](https://www.mobygames.com/images/shots/l/20018-gothic-windows-screenshot-the-game-begins-with-this-conversation.jpg)  
_**Fig. 1:** Dialogue Lines ([Source](https://www.mobygames.com/game/windows/gothic/screenshots/gameShotId,20018/))_

In Gothics Dialogue-System, everything revolves around _Information_-Instances and their _Conditions_.

## Dialogue Lines

Every Dialogue Line you can see in **_Fig. 1_** is an _Information_-Instance, which has the following properties:

- **Description** The text the player sees in the dialogue menu
- **Priority** Top to Bottom ordering
- **Is Permanent?** Whether the line is always shown and can be used multiple times.
- **Is Important?** Whether the Npc should start talking to the player automatically to tell him this information. Guards usually do this to stop you.
- **Condition-Function** If this script function returns `True`, this information is shown in the dialogue menu. Evaluated every time the Dialogue Menu is loaded.
- **Info-Function** Script function to be called when this information got selected in the dialogue menu.

Every time the dialogue menu is loaded, the _Condition_ function is called to
check whether the dialogue line should be available in the menu.

Whenever you as the Player choses a dialogue line in the menu, the _Hero_
remembers that you chose that line. This is because only the originator of the
conversation stores the selected information-lines.

Dialogue-lines already known by the player and not flagged as `Permanent` will not show up in the menu anymore.

!!! note

    If two Npcs would talk to each other, the one starting the conversation would
    also store information-lines they chose. However, since Gothic is a Single
    Player game, that never happens.

The scripts can then check whether the Hero knows a certain information [like so](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/Story/MISSIONS/DIA_GUR_1201_CorKalom.d#L255):

```c
// Equals to "Has the player ever chosen that dialogue line?"
if (Npc_KnowsInfo(hero,DIA_BaalOrun_GotWeed))
{
  // ... do something
}
```

## Saying something

Letting the Characters actually say something is done by enqueuing dialogue lines into the [Action Queue](ActionQueue.md) by calling `AI_Output` from the Info-Function like so:

```c
AI_Output(hero, self, "Info_Diego_Brief_15_00"); //Ich habe einen Brief für den obersten Feuermagier.
AI_Output(self, hero, "Info_Diego_Brief_11_01"); //So...?
AI_Output(hero, self, "Info_Diego_Brief_15_02"); //Ein Magier hat ihn mir gegeben, kurz bevor sie mich reingeworfen haben.
AI_Output(self, hero, "Info_Diego_Brief_11_03"); //Du kannst von Glück sagen, dass ich mich bei den Magiern nicht mehr blicken lassen kann. Jeder andere wird dir mit Freude für
```

Lets break it down:

- The first two parameters are who is saying the line to who. This is a dialogue between Diego and the Hero, so `self` refers to Diego in this case. Note how they switch back and forth!

- The third Parameter is a unique reference to the output-database, which stores the actual dialogue text and the WAV-file to play.

When each of these `AI_Outputs` is pushed into the [Action Queue](ActionQueue.md) of the talking character, the character which is being talked to explicitly checks whether a conversation is active and blocks its own Action-Queue while someone else says something.

!!! note

    The original workflow for these files incorporated parsing the comments behind the `AI_Output` and putting them into the database.

## Example - Drug Monopol

In this dialogue, the player gets asked by Cor Kalom to stop a Swampweed production of the New Camp. This is the [_Information_-Instance](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/Story/MISSIONS/DIA_GUR_1201_CorKalom.d#L379):

```c
INSTANCE Info_Kalom_DrugMonopol (C_INFO)
{
  npc         = GUR_1201_CorKalom;
  condition   = Info_Kalom_DrugMonopol_Condition;
  information = Info_Kalom_DrugMonopol_Info;
  permanent   = 0;
  description = "Hast DU noch eine Aufgabe für mich?";
};
```

Here, the condition checks, whether the player is joined the Swamp Camp and is now a Novice:

```c
FUNC INT Info_Kalom_DrugMonopol_Condition()
{
  if (Npc_GetTrueGuild(other)==GIL_NOV)
  {
    return 1;
  }

  // implicit return 0
}
```

When the Player selected the dialogue line of this _Information_-Instance, this function gets called:

```c
FUNC VOID Info_Kalom_DrugMonopol_Info()
{
  AI_Output (other, self,"Mis_1_Psi_Kalom_DrugMonopol_15_00"); // ...
  AI_Output (self, other,"Mis_1_Psi_Kalom_DrugMonopol_10_01"); // ...

  // ... <snip>
}
```

Once the Player has selected the dialogue line, the Hero remembers it. Other scripts can then check whether that dialogue line was ever chosen by the player via a call to `Npc_KnowsInfo`.

Dialogue-lines already known by the player and not flagged as `Permanent` will not show up in the menu anymore.

!!! note

    Notice how in Kor Kaloms `AI_Output` the two parameters are `other` and `self`, while the dialogue of Diego listed above explicitly used `hero`. I can imagine that is because Kor Kaloms Dialogue was made multiplayer-ready!
