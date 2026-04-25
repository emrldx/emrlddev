---
layout: post
title:  Adaptive Music
description: How I programmed sheet music that changes during gameplay
date:   2026-04-25 02:00:00 +0000
image:  '/images/AudacitySFXR.jpg'
tags:   [Game-Development, BTS, Coding, Music]
---

On load, the script runs:
``` py
func _ready():
	for child in get_children():
		if child is AudioStreamPlayer:
			child.volume_db += float(volume)
	
	var song = songList[adaptiveSongChosen]
	for setting in song:
		var audioSetting = AudioSoundSetting.new(setting[0],setting[1],setting[2],setting[3],setting[4],setting[5])
		audio_settings.append(audioSetting)
	var offBeatTrack = songOffBeatList[adaptiveSongChosen]
	for setting in offBeatTrack:
		var audioSetting = AudioSoundSetting.new(setting[0],setting[1],setting[2],setting[3],setting[4],setting[5])
		audio_settings_offBeat.append(audioSetting)
		
	if playSong:
		if adaptiveSongChosen=="Olympic":
			$Song.play()
		#elif ___ (it goes on)
	
	# Calculate time per beat
	time_per_beat = 60.0 / bpm

	# Create and start timer
	beat_timer.wait_time = time_per_beat
	offBeat_timer.wait_time = time_per_beat
	beat_timer.start()
	await get_tree().create_timer(time_per_beat/2).timeout
	offBeat_timer.wait_time = time_per_beat
	offBeat_timer.start()
```

Every time BeatTimer and OffBeatTimer fire, the following code is used:
``` py
func getCurrentBeat():
	return int(Time.get_ticks_msec() / 1000.0 / time_per_beat) % time_signature +1

func should_play_sound(c: int, b: int, t: int, p: int) -> bool:
	var effective_beat_number = (c - b) % t 
	var beats_per_play = t / p
	return effective_beat_number == 0 or effective_beat_number % beats_per_play == 0

func _on_beat():
	beatsElapsed+=1
	if ((beatsElapsed-1)%time_signature==0):
		barsElapsed+=1
	for setting in audio_settings:
		var current_beat = getCurrentBeat()
		if setting.playing and (setting.playOnBar == 1 or barsElapsed % setting.playOnBar == 0) and should_play_sound(current_beat,setting.beat,time_signature,setting.beats_per_bar):
			setting.audio_player.play()
			addSoundToDebugList(setting.audio_player.name)
		
func _on_off_beat():
	for setting in audio_settings_offBeat:
		var current_beat = getCurrentBeat()
		if setting.playing and (setting.playOnBar == 1 or barsElapsed % setting.playOnBar == 0) and should_play_sound(current_beat,setting.beat,time_signature,setting.beats_per_bar):
			setting.audio_player.play()

```