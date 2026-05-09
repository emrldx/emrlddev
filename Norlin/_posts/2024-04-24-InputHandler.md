---
layout: post
title:  Input Handler
description: How inputs are processed for quick actions
date:   2025-12-01 01:10:00 +0000
image:  '/images/MotionInput.png'
tags:   [Game-Development, BTS, Coding, Debugging,WIP]
---
WIP
![Right of Way]({{ "/images/Motion Input 236.png" | relative_url }})
![Right of Way]({{ "/images/Motion Input 623.png" | relative_url }})

InputHandler
``` py
extends Node
@onready var chara = get_parent()
var direction : Vector2 = Vector2.ZERO
var inputDirection : Vector2 = Vector2.ZERO;
var queuing := false #this is never directly set, as queuedActionName sets it
var queueStunned := false
var queueGatling := false
var queueWaitTime := 0.0:
	set(val):
		queueWaitTime = val
		$QueueWaitTimer.set_wait_time(val)
		$QueueWaitTimer.start()
var queuedActionName := "":
	set(val):
		queuedActionName = val
		if val != "":
			$QueueExpireTimer.start()
		#else:
		#	queueGatling = false
@export var currentInput : int
@export var previousInputs : Array
var previousInputsAtTimerStart : Array
@export var motionInput : int = 0:
	set(inputInNumbers):
		get_parent().motionInput = inputInNumbers
	get:
		return get_parent().motionInput
var previousDirection : Vector2 = Vector2.ZERO;
var holding = false
var parryCD := false
var enabled = true
@onready var stateMachine = get_parent().get_node("StateMachine")
@onready var currentState = stateMachine.currentState
@onready var audioHandler = chara.get_node("AudioHandler")

@onready var moveListFromStorage = chara.get_node("MoveListStorage").getListFromCharacter[chara.character]
@onready var fullMoveList = moveListFromStorage[0]
@onready var inputList = moveListFromStorage[1]
@onready var inputListAir = moveListFromStorage[2]

var inputHistory = []
var inputHistoryFrame := 0
var inputHistoryEnabled := true #true when the round starts, false when the round ends so that only the right stuff is considered, and so that the inputs are synced (instead of being thrown off when someone loads in before the other)
var inputHistoryReiterating := false
var inputHistoryReiteratingFrame := 0
var positionHistory = []
var velocityHistory = []
var directionHistory = []
var meterHistory = []
var stockHistory = []

var nearbyInputs = {
	"0":[0],
	"1":[1,2],
	"2":[2,1,3],
	"3":[3,2],
	"4":[4],
	"5":[5],
	"6":[6],
	"7":[7],
	"8":[8],
	"9":[9]
}

func getInputName(event): #TODO get rid of ui inputs
	var all_actions: Array[StringName] = InputMap.get_actions()
	# Filter and find the actions that match the current event
	var matching_actions: Array[StringName] = all_actions.filter(func(action_name: StringName): 
		return event.is_action(action_name)
	)
	if matching_actions.size() <=0:
		return
	#print(matching_actions)
	var inputName = matching_actions[0]
	#print_debug(inputName)
	if (inputName.contains("p2") and chara.flipped == 1) or (not inputName.contains("p2") and chara.flipped == -1):
		return
	if inputName.contains("p2"):
		inputName = inputName.substr(2)
	return(inputName)

func updateStateInputs():
	if chara.aiMode:
		return
	for state in stateMachine.states:
		state.inputDirection = inputDirection
	
func _input(event : InputEvent):
	if get_parent().flipped == 1 and event.is_action_pressed("menu"):
		get_tree().current_scene.menu()
	if chara.aiMode or not (event is InputEventKey) or inputHistoryReiterating or not enabled:
		return
		
	if event.is_action_pressed("debug"):
		if get_parent().aiMode:
			get_parent().get_node("AIHandler").setLabelVisibility()
	elif event.is_action_pressed("reset"):
		get_tree().current_scene.reset()
	elif(Input.is_action_just_pressed("override")) and chara.flipped == 1:
		get_tree().current_scene.overrideCamera(chara)
		return
	if (Input.is_action_just_pressed(inputs("parry"))) and motionInput == 632146 and currentState.canDoActionName("Priority") and chara.superStocks > 0 and chara.meter > 33:
		get_tree().current_scene.summonProjectile(chara,"Disengage",Vector2(-5,0),0.0,Vector2(-1,1))
		get_tree().current_scene.vfx(chara,"SuperStrike",Vector2(-10,3),false,0.0,Vector2(1,1))
		#get_tree().current_scene.summonProjectile(chara,"Super",Vector2(-50,0),0.0,Vector2(-1,1))
		#chara.activatePriority()
		#get_tree().current_scene.usePriority(chara)
		currentState.doActionName("Priority")
		return
		
	
	var inputName = getInputName(event)
	if event != null and inputName != null and inputHistoryEnabled and inputHistoryFrame > 0:
		#print((chara.flipped ==1 and not inputName.contains("p2")))
		#print((chara.flipped ==-1 and inputName.contains("p2")))
		#if inputHistoryEnabled and inputHistoryFrame > 0 and ((chara.flipped ==1 and not inputName.contains("p2")) or (chara.flipped ==-1 and inputName.contains("p2"))):
		#if inputHistoryFrame>inputHistory.size():
		#	inputHistory.append([[event,inputName]]) #first input for a given frame
		#else:
		#print(inputHistory)
		#print(inputHistory.size())
		if inputHistory[inputHistoryFrame-1]==[]:
			inputHistory[inputHistoryFrame-1] = [[event,inputName]]
		else:
			#inputHistory[inputHistoryFrame-1] = [inputHistory[inputHistoryFrame-1],[event,inputName]] #add inputs to this frame, incase the player presses hella buttons at once
			inputHistory[inputHistoryFrame-1].append([event,inputName])
	
	
	processInput(event, inputName)


func processInput(event, inputName):
	#if event ==[] or inputName==[]: #TODO this is such a fucking spaghetti code workaround
	#	return
	if not parryCD and (inputName=="right" or inputName=="p2right") and stateMachine.currentState==stateMachine.currentState.neutralState:
		parryCD = true
		$ForwardParryCD.start()
		#var progressBar = get_parent().get_node("ParryProgressBar")
		#if progressBar != null:
		#	progressBar.begin(9)
		chara.canForwardParry = true
		await get_tree().create_timer(9 * .0167).timeout
		chara.canForwardParry = false
		
	if event.is_action_pressed(inputs("parry")) and motionInput == 214:
		get_tree().current_scene.get_node("CharacterHandler").spendMeterForStock(chara)
	#print("Motion input: "+str(motionInput))
	#direction = Input.get_vector(inputs("left"), inputs("right"), inputs("up"),inputs("down")).sign()
	
	"""
	if inputHistoryReiterating:
		if inputName.contains("right"): #TODO this is a shitty workaround because i dont know how to get the input direction otherwise with just the events
			inputDirection = Vector2i(1,0)
		elif inputName.contains("left"):
			inputDirection = Vector2i(-1,0)
		elif inputName.contains("down"):
			inputDirection = Vector2i(0,1)
		elif inputName.contains("up"):
			inputDirection = Vector2i(0,-1)
	"""
	
	var all_actions: Array[StringName] = InputMap.get_actions()
	# Filter and find the actions that match the current event
	var matching_actions: Array[StringName] = all_actions.filter(func(action_name: StringName): 
		return event.is_action(action_name)
	)
	if matching_actions.size() <=0:
		return
	#print(matching_actions)
	var motionInputName = matching_actions[0]+str(motionInput)
	inputName = matching_actions[0]+str(Vector2i(inputDirection)) #TODO I wonder if this will cause problems later?
	if not searchAction(motionInputName):
		searchAction(inputName)
		

func searchAction(inputName):
	if (inputName.contains("p2") and chara.flipped == 1) or (not inputName.contains("p2") and chara.flipped == -1):
		return false
	if inputName.contains("p2"):
		inputName = inputName.substr(2)
	#print(inputName)
	var actionName
	if inputList.has(inputName):
		actionName = inputList[inputName]
	else:
		#print_debug("Inputted event could not be found! "+inputName)
		return false
	if fullMoveList.has(actionName):
		if queueStunned:
			queuedActionName = actionName
			return true
		elif currentState.canDoActionName(actionName):
			currentState.doActionName(actionName)
		#queuedActionName = ""
			return true
	return false

func _process(delta: float) -> void:
	currentInput = 0 #TODO the following is a big batch of bullshit
	
	if (Input.is_action_just_pressed(inputs("up")) and inputDirection.x == -1) or (Input.is_action_just_pressed(inputs("left")) and inputDirection.y == -1):
		currentInput = 7
	elif (Input.is_action_just_pressed(inputs("up")) and inputDirection.x == 1) or (Input.is_action_just_pressed(inputs("right")) and inputDirection.y == -1):
		currentInput = 9
	elif (Input.is_action_just_pressed(inputs("down")) and inputDirection.x == -1) or (Input.is_action_just_pressed(inputs("left")) and inputDirection.y == 1):
		currentInput = 1
	elif (Input.is_action_just_pressed(inputs("down")) and inputDirection.x == 1) or (Input.is_action_just_pressed(inputs("right")) and inputDirection.y == 1):
		currentInput = 3
	elif Input.is_action_just_pressed(inputs("up")) or ((Input.is_action_just_released(inputs("left")) or Input.is_action_just_released(inputs("right"))) and inputDirection.y == -1):
		currentInput = 8
	elif Input.is_action_just_pressed(inputs("down")) or ((Input.is_action_just_released(inputs("left")) or Input.is_action_just_released(inputs("right"))) and inputDirection.y == 1):
		currentInput = 2
	elif Input.is_action_just_pressed(inputs("left")) or ((Input.is_action_just_released(inputs("up")) or Input.is_action_just_released(inputs("down"))) and inputDirection.x == -1):
		currentInput = 4
	elif Input.is_action_just_pressed(inputs("right")) or ((Input.is_action_just_released(inputs("up")) or Input.is_action_just_released(inputs("down"))) and inputDirection.x == 1):
		currentInput = 6
	
	if currentInput != 0:
		addToInputsArray(currentInput)
		$ExpireInputs.stop()
		previousInputsAtTimerStart = previousInputs
		$ExpireInputs.set_wait_time(11 * .0167)
		$ExpireInputs.start()
	
	if currentState==currentState.neutralState or currentState==currentState.blockState:
		if inputDirection.x==-1:
			chara.blocking = true
			if inputDirection.y==1:
				chara.blockingLow = true
			else:
				chara.blockingLow = false
		else:
			chara.blocking = false
			chara.blockingLow = false
	else:
		chara.blocking = false
		chara.blockingLow = false
	if not queueStunned and queuedActionName != "":
		if currentState.canDoActionName(queuedActionName):
			currentState.doActionName(queuedActionName)
			queuedActionName = ""
	"""
	if queuing:
		if (not (queueWaitTime != 0.0 or queueStunned)) and currentState.canDoActionName(queuedActionName):
			print("Can do queued action!")
			currentState.doActionName(queuedActionName)
			if queueGatling:
				currentState.gatling()
			queuedActionName = ""
		"""
		
func _physics_process(delta: float) -> void:
	currentState = stateMachine.currentState
	
	if chara.aiMode:
		return
	if not inputHistoryReiterating and not chara.aiMode:
		inputDirection = Vector2i(
			int(sign(Input.get_action_strength(inputs("right"))) - sign(Input.get_action_strength(inputs("left")))),
			int(sign(Input.get_action_strength(inputs("down"))) - sign(Input.get_action_strength(inputs("up"))))
		)
	
	if inputHistoryEnabled:
		inputHistory.append([])
		inputHistoryFrame+=1
		positionHistory.append(chara.global_position)
		velocityHistory.append(chara.velocity)
		directionHistory.append(inputDirection)
		meterHistory.append(chara.meter)
		stockHistory.append(chara.superStocks)
	if inputHistoryReiterating:
		inputHistoryReiteratingFrame+=1
		if inputHistoryReiteratingFrame >=inputHistoryFrame:
			doneReiterating()
			return
		for input in inputHistory[inputHistoryReiteratingFrame]:
			if input != null and input != []: 
				#print_debug(str(input)+"   "+str(input[0]))
				#print(input)
				processInput(input[0],input[1])
		chara.velocity = velocityHistory[inputHistoryReiteratingFrame]
		inputDirection = directionHistory[inputHistoryReiteratingFrame]
		chara.meter = meterHistory[inputHistoryReiteratingFrame]
		chara.superStocks = stockHistory[inputHistoryReiteratingFrame]
	updateStateInputs()

func addToInputsArray(currentInput1):
	previousInputs.append(currentInput1)
	#print("Previous inputs: "+str(previousInputs))
	previousDirection = inputDirection
	if previousInputs.size() >= 11:
		previousInputs.remove_at(0)
	#print(previousInputs)
	checkForMotionInputs()
	
func checkForMotionInputs():
	#if previousInputs[len(previousInputs)-1] == 6 and previousInputs[len(previousInputs)-2] == 3 and previousInputs[len(previousInputs)-3] == 2:
	if getNearbyInput(6,6) and getNearbyInput(5,3) and getNearbyInput(4,2) and getNearbyInput(3,1) and getNearbyInput(2,4) and getNearbyInput(1,6):
		motionInput = 632146
		$ExpireMotionInput.start()
	elif getNearbyInput(6,4) and getNearbyInput(5,1) and getNearbyInput(4,2) and getNearbyInput(3,3) and getNearbyInput(2,6) and getNearbyInput(1,4):
		motionInput = 412364
		$ExpireMotionInput.start()
	elif getNearbyInput(5,4) and getNearbyInput(4,1) and getNearbyInput(3,2) and getNearbyInput(2,3) and getNearbyInput(1,6):
		motionInput = 41236
		$ExpireMotionInput.start()
	elif getNearbyInput(3,2) and getNearbyInput(2,3) and getNearbyInput(1,6):
		if motionInput == 236:
			motionInput = 236236
		else:
			motionInput = 236
		$ExpireMotionInput.start()
	elif getNearbyInput(3,2) and getNearbyInput(2,1) and getNearbyInput(1,4):
		if motionInput == 214:
			motionInput = 214214
		else:
			motionInput = 214
		$ExpireMotionInput.start()
		#print("Motion input! "+str(motionInput))
	#elif getNearbyInput(5,4) and getNearbyInput(4,4) and getNearbyInput(3,1) and getNearbyInput(2,6) and getNearbyInput(1,6):
	#elif getNearbyInput(6,4) and getNearbyInput(5,1) and getNearbyInput(4,2) and getNearbyInput(3,3) and getNearbyInput(2,6) and getNearbyInput(1,6):
	elif getNearbyInput(5,2) and getNearbyInput(4,1) and getNearbyInput(3,4) and getNearbyInput(2,6) and getNearbyInput(1,6):
		#motionInput = 412366
		motionInput = 21466
		$ExpireMotionInput.start()
		if stateMachine.currentState.canDoActionName("Dash"):
			checkForward()
	elif getNearbyInput(5,2) and getNearbyInput(4,3) and getNearbyInput(3,6) and getNearbyInput(2,4) and getNearbyInput(1,4):
		motionInput = 23644
		$ExpireMotionInput.start()
		if stateMachine.currentState.canDoActionName("Backdash"):
			checkBackward()
	elif getNearbyInput(3,6) and getNearbyInput(2,2) and getNearbyInput(1,3):
		motionInput = 623
		$ExpireMotionInput.start()
	elif getNearbyInput(2,6) and getNearbyInput(1,6):
		#motionInput = 66
		doActionName("Dash")
	elif getNearbyInput(2,4) and getNearbyInput(1,4):
		#motionInput = 44
		doActionName("Backdash")

func inputs(input):
	if get_parent().flipped == -1:
		return "p2"+input
	return input


func getNearbyInput(indexSubtract,desiredInput):
	if len(previousInputs)<indexSubtract:
		return false
	#print(str(previousInputs[len(previousInputs)-indexSubtract]))
	#print(nearbyInputs[str(previousInputs[len(previousInputs)-indexSubtract])])
	return nearbyInputs[str(previousInputs[len(previousInputs)-indexSubtract])].has(desiredInput)

func checkForward():
	stateMachine.currentState.doActionName("Dash")
	chara.get_node("SFXDashDodge").play()
	chara.dodging = true
	chara.dodgeAnim()
func checkBackward():
	stateMachine.currentState.doActionName("Backdash")
	chara.get_node("SFXDashDodge").play()
	chara.dodging = true
	chara.dodgeAnim()


func doActionName(namee):
	previousInputs.clear()
	motionInput = 0
	if stateMachine.currentState.canDoActionName(namee):
		stateMachine.currentState.doActionName(namee)

		
func _on_expire_motion_input_timeout():
	motionInput = 0


func _on_forward_parry_cd_timeout() -> void:
	parryCD = false


func _on_expire_inputs_timeout() -> void:
	if previousInputs == previousInputsAtTimerStart:
		previousInputs.clear()

func block():
	stateMachine.currentState.block()




func _on_queue_expire_timer_timeout() -> void: #time for a queued action to go away (not queue anymore cause the window expired)
	queuedActionName = ""


func _on_queue_wait_timer_timeout() -> void: #time for a queued action to wait before it happens. if this is 0.0, then the action comes out as soon as possible, or another input can override it. if not 0.0, the queued action HAS to happen. cool!
	queueWaitTime = 0.0

func resetInputHistory():
	inputHistoryEnabled = false
	inputHistory = []
	directionHistory = []
	positionHistory = []
	velocityHistory = []
	meterHistory = []
	stockHistory = []
	inputHistoryFrame = 0

func startInputHistory():
	inputHistoryEnabled = true
	inputHistory = []
	inputHistoryFrame = 0

func disableInputHistory():
	inputHistoryEnabled = false

func reiterateInputHistory(frameOffset):
	inputHistoryEnabled = false
	#print(inputHistoryFrame)
	if frameOffset < 0 or frameOffset >= inputHistoryFrame:
		var difference = abs(frameOffset-inputHistoryFrame)
		chara.position = positionHistory[0]
		await get_tree().create_timer(difference * .0167).timeout
		inputHistoryReiteratingFrame = 0
		print_debug("INPUT HISTORY REITERATING FRAME MESSED UP because of frame offset or inputHistoryFrame! It was set to 0...")
	else:
		inputHistoryReiteratingFrame = inputHistoryFrame - frameOffset
		chara.position = positionHistory[inputHistoryReiteratingFrame]
	inputHistoryReiterating = true

func doneReiterating():
	inputHistoryReiterating = false
	resetInputHistory()
	chara.doneReiterating()
```