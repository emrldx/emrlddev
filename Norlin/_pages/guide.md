---
layout: page
title:  Right of Way Guide
image:  '/images/memes/GuyShrugging.png'
permalink: /guide/
---
# Major System Mechanic: "Right of Way"

In most fighting games, a "hit" just lowers a health bar. In *Right of Way*, a hit is the end. This high-stakes environment is governed by two core systems.

### The Rule of Priority
Borrowed from fencing, right of way goes to the player on the offensive. If both players hit each other at the exact same time, the one with priority wins the point. This encourages calculated aggression rather than mindless retreating.

# Blocking
Hold backward to block, which negates all damage.
Blocking is punished by losing meter, and the attacker can cancel attacks into higher-priority attacks (see below*). This is to discourage blocking as a defense mechanic, preferring parrying.
{% include video.html url="/videos/Blockstring.mp4" %}

Be careful, blockstrings can be dangerous to defend against!
{% include video.html url="/videos/ShoBlockstring.mp4" %}

{: .tip }
While blocking, your blade can still be hit by beats!



# Beats
In fencing, beats apply miniscule pressure. With a slight tap, you hint to the opponent that you might attack momentarily. Or, you may do a circle-6 parry into a lunge. Maybe you'll fleche. Maybe you'll do nothing. Who knows? It's all one big mind game.
In Right of Way, beats act similarly. They can be confused with actual attacks, leading the opponent into committing to a huge attack--but your beat was quick, so you are in neutral, therefore you can parry. Oops, big attack coming your way, you parry-riposte and get a point.
{% include video.html url="/videos/Beat.mp4" %}
Also, beats siphon meter. Not only do they give meter, but they steal the same amount from the opponent. Typically, meter gain is generous (it comes from simply having right of way). Therefore, slow your opponent's progress with a quick beat. 
...until you do that too much, and they counter with a bind.
# Binds
Binds can only be performed against an opponent's beat attack. Thus, binds act more like a counter than a grab.
{% include video.html url="/videos/Bind.mp4" %}
They have 9 frames of advantage and keep the opponent relatively close. This enables okizeme--the state of applying pressure while the opponent is 
immobile. The opponent has to defend immediately once out of stun--except if you bait a parry, of course.
##### Conditioning
Condition the opponent to beat a lot, then counter with a bind. Follow up with a lunge. Land enough binds-then-lunges, and your opponent will be conditioned to either not beat, or to parry after getting binded. For the former, you get more meter. For the latter, you successfully trained your opponent to do what you want, and you know exactly that. Land another bind, and when they try to parry, you simply... do nothing. Simply delay your attack. Bait out the parry, then punish. Free point.

Throws are very similar.
# Throws
There is no grappling in fencing. Yet, as with blocking, throwing is a staple of fighting games. If the opponent only defends, how will you force your way through without a throw that goes through block? Actually, in Right of Way, if you push the opponent down and off the strip, you get a point, so defense isn't a problem to deal with... uhh
Ignoring that, throws felt necessary in the end. Plus, this game is a mix of HEMA fencing too (HEMA longsword fencer pending), and grappling is allowed in that sport. So, throws were inevitable.
You get 5 frames of advantage when you throw the opponent, and more importantly, they go sliding down the strip. This gives you control of space on the strip. For example, if pushed to your end of the strip, you can close distance, throw the opponent, then push forward to avoid going off-strip.
{% include video.html url="/videos/Throw.mp4" %}

### Binds vs Throws
You follow the opponent more with a bind than a throw, but the knockback is greater with a throw; binds have more frame advantage and keep you closer, thus they are  better for okizeme, meanwhile throws are better for stealing space.
{% include video.html url="/videos/ThrowVsBind.mp4" %}
Both are great for conditioning and punishing.


*
# Canceling
The order is as follows, from lowest to greatest:
1. Beat
2. Cut normal attack
3. Cut special attack
4. Lunge normal attack
5. Lunge special attack
For example, during a blockstring, you can cancel from a normal Cut into a Lunge, but once you do Lunge, you cannot cancel into a Cut.
You can always cancel a beat. However, for the rest, you either have to hit the opponent's block, or activate PRIORITY.
{% include video.html url="/videos/Blockstring.mp4" %}

# PRIORITY
An install super that rotates the screen and fills it with your player's color.
![PRIORITY]({{ "/images/guiltyGear/PRIORITY4.png" | relative_url }})

During PRIORITY, you retain right of way, no matter what you do. In addition, you can cancel your attacks WITHOUT needing a blockstring.
{% include video.html url="/videos/PriorityExample.mp4" %}
Beware, your opponent can also activate PRIORITY to steal yours!
{% include video.html url="/videos/PrioritySwitch.mp4" %}
back-and-forth...
{% include video.html url="/videos/PrioritySwitchALot.mp4" %}

# Super Advances/Retreats
{% include video.html url="/videos/SuperAdvanceExample.mp4" %}

{: .note }
*note: this is intended for playtesters.*

# Controls
Controllers are supported! However, motion inputs are buggy/impossible on joystick (dw, almost no moves use them, and I'll fix this soon)
Shift or double tap a direction - DASH
Up or down: change TEMPO

Player 1:
WASD
Y - BEAT
U - CUT
I - LUNGE
O - PARRY
J - TAUNT

Player 2:
Arrow Keys
M - BEAT
. - CUT
, - LUNGE
/ - PARRY
TAUNT - ;

{: .important }
Notation that will be used from here on out
### Numpad Notation:
Directions coincide with the number pad;
4 is backward, 6 is forward, 8 is up, 2 is down,
1379 are for in-between.
236: Quarter-circle-forward
214: Quarter-circle-backward


# SYSTEM MECHANICS

Use 1 stock during an attack animation:
Quarter-circle-forward PARRY: Super Advance
Quarter-circle-back PARRY: Super Retreat

When at 3/4 meter: Quarter-circle-back PARRY to gain 1 stock

## Beats & Binds
Beat the opponent's blade to apply pressure & gain meter;
if landed, you can cancel into an attack.
Opponent keeps beating your blade? Do a bind!
Down-forward BEAT to counter a regular beat;
if landed, you push them down and have okizeme (advantage)

## Disengage
Press PARRY immediately after your attack gets parried.
Pressing too early will lock you out of this (as heard by a beep)

## Dodges
Fencing footwork - Dashes gain i-frames
Check Forward: 214 66 (Quarter-circle-back Forward-Forward)
Check Backward 236 44 (Quarter-circle-back Backward-Backward)

## Taunts
Get through an entire taunt for a free stock
(currently, only available for the Foilist)


# Movelists
### Foilist
Beats: 5B, 6B, 3B (bind)
Normals: 5C, 5L
Supers: (require 1 stock)
214 LUNGE (2-3 beats, lunge)
236 LUNGE (2 dodges, lunge)

### Sho
Beats: 5B, 3B (bind)
236 CUT: fireball (deactivated for now)
236 LUNGE: three-hitting kick
Supers: (require 1 stock)
236236 LUNGE (SF3 Ken's Super Art 3)