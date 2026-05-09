---
layout: post
title:  Right-of-way handler
description: How the rule of priority was programmed
date:   2026-05-01 01:01:00 +0000
image:  '/images/marketing/LogoBoxRed.png'
tags:   [Fencing,BTS,Coding]
---
Before going into the code, here's an overview of what priority is:

“**Right of way**” in fencing is the rule that decides *which fencer’s touch scores first* when **both** fencers hit at about the same time.

- Fencing is fast, so officials use “right of way” (often called **priority**) to determine who is on the attack.
- When both fencers hit each other nearly simultaneously, that is called a double-touch.
- **Only the fencer who had right of way scores from a double-touch**.
- Right of way is lost by losing initiative (e.g. walking backward, no longer on the attack)

## How right of way is determined typically
Right of way generally belongs to the fencer who is considered to be:

### 1) **Attacking**
Typically the person who initiates a valid **attack** (for example, lunging/advancing with correct fencing action).
- If their attack is judged to be the real, ongoing action, they usually have priority.
- If the opponent tries to score by attacking or countering, the timing matters.

### 2) Defending successfully
In many situations, a defender can gain the advantage by:
- Stopping the attack in a valid way (e.g., with a legitimate parry/disengage sequence, depending on the situation).
- Examples are parrying the opponents blade or moving out of range of the opponent's attack
- Suddenly, the defender has right of way

### 3) Counter-attacks and ripostes
- If the opponent parries or blocks correctly, the initiative can shift, and then the next valid action may create new priority.
- Note: this means that, for example, if you parry the opponent but walk backward, you don't have right of way.
- Different weapons handle this with slightly different timing nuances, but the principle is the same: the referee decides who had the initiative when the touch happened.


<hr/>
WIP
# The Code
``` py
extends Node
@onready var eventHandler = get_parent().get_node("EventHandler")
@onready var characterHandler = get_parent().get_node("CharacterHandler")
var plr : CharacterBody2D
var plrAI : Node
var plr1 : CharacterBody2D
var plr1AI : Node
var rightOfWay : CharacterBody2D:
	set(roW):
		var atRoundStart = get_parent().atRoundStart
		var priority = get_parent().priority
		if atRoundStart != 1 and atRoundStart != 2 and atRoundStart != 3 and priority == null and rightOfWay != roW:
			characterHandler.roW = roW
			eventHandler.roW = roW
			updateHUD(rightOfWay,roW)
			if plrAI != null:
				plrAI.roW = roW
			if plr1AI != null:
				plr1AI.roW = roW
			if roW == null:
				rightOfWay = roW
				plr.rightOfWay = false
				plr1.rightOfWay = false
				get_parent().rightOfWay = rightOfWay
			else:
				if rightOfWay != null:
					rightOfWay.rightOfWay = false
				rightOfWay = roW
				rightOfWay.rightOfWay = true
				gracePeriodCountdown = gracePeriod
				loseRoWCount = 0
				loseRoWCount1 = 0
			get_parent().rightOfWay = rightOfWay

var loseRoWCount : float
var loseRoWCount1 : float
var loseThreshold : float = 90 * .0167
var plrPrevX : float
var plr1PrevX : float
var touch := Vector2(0,0)
var wentPast = false
var plrPosition : float
var plr1Position : float
var RoWAttacking = false
var gracePeriod := 30 * .0167
var gracePeriodCountdown = gracePeriod

func _process(delta):
	var atRoundStart = get_parent().atRoundStart
	var waiting = get_parent().waiting
	gracePeriodCountdown -= delta

	if atRoundStart == 3 or atRoundStart == 5 and not waiting:
		var plrState = plr.get_node("StateMachine").currentState
		var plr1State = plr1.get_node("StateMachine").currentState
		
		plrPosition = plr.position.x
		plr1Position = plr1.position.x
		
		var plrDoingAction = plrState==plrState.attackingState or plrState==plrState.throwState or plrState==plrState.beatState or plrState==plrState.parryState or plrState==plrState.superState
		var plr1DoingAction = plr1State==plr1State.attackingState or plr1State==plr1State.throwState or plr1State==plr1State.beatState or plr1State==plr1State.parryState or plr1State==plr1State.superState
		# plr faces right: advancing = x increasing, retreating = x decreasing
		# plr1 faces left: advancing = x decreasing, retreating = x increasing
		var plrAdvancing = plrPosition > plrPrevX or plrDoingAction and gracePeriodCountdown <= 0.0
		var plrRetreating = plrPosition <= plrPrevX and not plrDoingAction and gracePeriodCountdown <= 0.0
		var plr1Advancing = plr1Position < plr1PrevX or plr1DoingAction and gracePeriodCountdown <= 0.0
		var plr1Retreating = plr1Position >= plr1PrevX and not plr1DoingAction and gracePeriodCountdown <= 0.0
		if plrRetreating:
			loseRoWCount += 5*delta
		elif plrPosition == plrPrevX:
			loseRoWCount += 1*delta
		else:
			loseRoWCount = 0

		if plr1Retreating:
			loseRoWCount1 += 5*delta
		elif plr1Position == plr1PrevX:
			loseRoWCount1 += 1*delta
		else:
			loseRoWCount1 = 0
			
		if plrAdvancing:
			loseRoWCount=0
		if plr1Advancing:
			loseRoWCount1=0
		
		#if not RoWAttacking:
		if plrRetreating and rightOfWay == plr and loseRoWCount>=loseThreshold:
			rightOfWay = null
		elif plr1Retreating and rightOfWay == plr1 and loseRoWCount1>=loseThreshold:
			rightOfWay = null
		if plrAdvancing and rightOfWay != plr and not plr1Advancing:
			rightOfWay = plr
			setZIndex(plr, plr1, false)
		elif plr1Advancing and not plrAdvancing and rightOfWay != plr1:
			rightOfWay = plr1
			setZIndex(plr1, plr, false)

		if rightOfWay != null:
			characterHandler.addMeter(rightOfWay, .15)

		plrPrevX = plr.position.x
		plr1PrevX = plr1.position.x
	processHUD()

func setZIndex(higherPlr:CharacterBody2D, lowerPlr:CharacterBody2D, override: bool):
	higherPlr.z_index = 1
	lowerPlr.z_index = 0

# Called when a player begins or ends an attack action
func playerAttacks(player: CharacterBody2D, over: bool):
	var atRoundStart = get_parent().atRoundStart
	if atRoundStart == 5:
		if not over:
			# Attack initiated — only claim RoW if no one has it, or this player already has it
			if rightOfWay == null or rightOfWay == player:
				rightOfWay = player
				RoWAttacking = true
				if plr == player:
					setZIndex(plr, plr1, false)
				else:
					setZIndex(plr1, plr, false)
		else:
			# Attack ended (whiff / action over) — attacker loses RoW
			if rightOfWay == player:
				RoWAttacking = false
				rightOfWay = null

func someoneAction(player, enemy):
	if rightOfWay == null:
		rightOfWay = player
		gracePeriodCountdown = gracePeriod
		
func someoneSteal(player, enemy):
	if rightOfWay == enemy:
		rightOfWay = player
		gracePeriodCountdown = gracePeriod

# Called when player successfully beats (knocks aside) opponent's blade
func someoneBeat(player, beated):
	if rightOfWay == beated:
		rightOfWay = player
		RoWAttacking = false
		gracePeriodCountdown = gracePeriod

		
# Called when player successfully parries opponent's attack
func someoneParried(player, parried):
	# A parry deflects the attack — the parrying fencer earns RoW
	if rightOfWay == parried:
		RoWAttacking = false
		rightOfWay = player
		gracePeriodCountdown = gracePeriod

func updateHUD(gainer,loser):
	get_parent().HUD.changeRoW(gainer,loser)
func processHUD():
	get_parent().HUD.setGracePeriod(gracePeriod,gracePeriodCountdown)
```
