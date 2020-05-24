# Full List of Gothic Event-Message Types and Subtypes

Taken from [REGoth-bs](https://github.com/REGoth-project/REGoth-bs/blob/master/src/AI/EventMessage.hpp).

- **Weapon**

  - DrawWeapon
  - DrawWeapon1
  - DrawWeapon2
  - RemoveWeapon
  - RemoveWeapon1
  - RemoveWeapon2
  - ChooseWeapon
  - ForceRemoveWeapon
  - Attack
  - EquipBestWeapon
  - EquipBestArmor
  - UnequipWeapons
  - UnequipArmor
  - EquipArmor

- **Movement**

  - RobustTrace
  - GotoPos
  - GotoVob
  - GoRoute
  - Turn
  - TurnToPos
  - TurnToVob
  - TurnAway
  - Jump
  - SetWalkMode
  - WhirlAround
  - Standup
  - CanSeeNpc
  - Strafe
  - GotoFP
  - Dodge
  - BeamTo
  - AlignToFP

- **Attack**

  - AttackForward
  - AttackLeft
  - AttackRight
  - AttackRun
  - AttackFinish
  - Parade
  - AimAt
  - ShootAt
  - StopAim
  - Defend
  - AttackBow
  - AttackMagic

- **Manipulate**

  - TakeVob
  - DropVob
  - ThrowVob
  - Exchange
  - UseMob
  - UseItem
  - InsertInteractItem
  - RemoveInteractItem
  - CreateInteractItem
  - DestroyInteractItem
  - PlaceInteractItem
  - ExchangeInteractItem
  - UseMobWithItem
  - CallScript
  - EquipItem
  - UseItemToState
  - TakeMob
  - DropMob

- **Conversation**

  - PlayAniSound
  - PlayAni
  - PlaySound
  - LookAt
  - Output
  - OutputMonolog
  - OutputEnd
  - Cutscene
  - WaitTillEnd
  - Ask
  - WaitForQuestion
  - StopLookAt
  - StopPointAt
  - PointAt
  - QuickLook
  - PlayAni_NoOverlay
  - PlayAni_Face
  - ProcessInfos
  - StopProcessInfos
  - OutputSVM_Overlay
  - SndPlay
  - SndPlay3d
  - PrintScreen
  - StartFx
  - StopFx

- **Magic**

  - Open
  - Close
  - Move
  - Invest
  - Cast
  - SetLevel
  - ShowSymbol
  - SetFrontSpell
  - CastSpell
  - Ready
  - Unready

- **Mob**
  - StartInteraction
  - StartStatechange
  - EndInteraction
  - Unlock
  - Lock
  - Callscript
