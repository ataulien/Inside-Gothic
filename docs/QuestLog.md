# Tracking active Quests

![](https://www.mobygames.com/images/shots/l/162052-gothic-windows-screenshot-and-of-course-the-questlog.jpg)  
**_Fig. 1:_** Quest Log ([Source](https://www.mobygames.com/game/windows/gothic/screenshots/gameShotId,162052/))

Tracking active quests is handled a bit weird by Gothic. Scripts have
write-access to the players Quest-Log, but they cannot read back the quest
status. Thus, a global boolean variable was created for most quests to track
whether its active. Additionally, the Quest-Log is [kept in
sync](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/Story/MISSIONS/DIA_Bau_935_Homer.d#L110):

```c
Homer_DamLurker = LOG_RUNNING; // Global variable

Log_CreateTopic   (CH1_DamLurker, LOG_MISSION);
Log_SetTopicStatus(CH1_DamLurker, LOG_RUNNING);
```

I can't think of a reason other than the Quest-Log being implemented much later into development for this to make sense.

## Diary

In the game, the Quest Log is actually the hero's diary. On script-side, the functions

- **`Log_CreateTopic`:** Add Topic and choose between Mission or Note
- **`Log_SetTopicStatus`:** Set a Topic to Running, Success, Failed, Obsolete
- **`Log_AddEntry`:** Add a piece of text to a Topic

are used to update the diary.

Depending on the topics status, they are sorted into the categories.
Interestingly, the `Obsolete` categories is actually obsolete and not used by
the game. Topics using that category will not show up in the Diary.

!!! note

    The Diary is read-only, scripts are using global variables to manually keep track whether a quest is active.

    A full list of all quest topic names can be found [in the scripts](https://github.com/GothicII/GOTHIC-MOD-Development-Kit/blob/4071a70b3a8943e374ccbf886986e494b4be0858/gothic/_work/data/Scripts/content/Story/Log_Constants.d).
