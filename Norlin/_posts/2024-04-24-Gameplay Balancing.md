---
layout: post
title:  Gameplay Balancing
description: How do you translate fencing into video game form?
date:   2026-04-24 00:00:00 +0000
image:  '/images/me/me_7.jpg'
tags:   [BTS]
---
How do you balance a fighting game that has no healthbar?

# Blocking
Most of development, I resisted the mechanic of blocking. *Right of Way* should have NO blocking, because in fencing, if your opponent's blade comes at you, are you going to hold backwards and Nmagically not get hit? Of course not! If you are undoubtedly going to get hit, guess what? That's your fault. You can't just hold down-back when the opponent lunges at your feet. Instead, ask yourself, "Why are my feet so close to his blade?"  
However... playtesters beat *Right of Way* by spamming attacks. The game dwindled into the following cycle:
1. Round start
2. Walk forward
3. Press an attack button (either one)
4. Hit opponent
5. Hope you had right of way, because you both will hit each other
6. Round over, reset to round start, repeat  
That's brainless and easy. In contrast, parrying is narrow and difficult. Most players only grow frustrated at the attempt to react to attacks and parry accordingly. Meanwhile, they could just run up and attack. When the opponent also struggles to defend himself, the attack is bound to hit--causing a self-reinforcing perpetual loop. Players quickly learn to simply not parry.  
They also learn that the game isn't fun.  
It can only be fun if the player can defend himself. Thus, blocking is a necessary evil.
{% include video.html url="/videos/Blocking.mp4" %}

Blocking is punished by losing meter, and the attacker can cancel attacks into higher-priority attacks (see below*). This is to discourage blocking as a defense mechanic, preferring parrying.
{% include video.html url="/videos/Blockstring.mp4" %}
Also, your blade can still be hit by beats.

# Canceling
The order is as follows, from lowest to greatest:
1. Cut normal attack
2. Cut special attack
3. Lunge normal attack
4. Lunge special attack
For example, during a blockstring, you can cancel from a normal Cut into a Lunge, but once you do Lunge, you cannot cancel into a Cut.

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

# PRIORITY
An install super that rotates the screen and fills it with your player's color.
During PRIORITY, you retain right of way, no matter what you do. In addition, you can cancel your attacks WITHOUT needing a blockstring.
{% include video.html url="/videos/PriorityExample.mp4" %}

![PRIORITY]({{ "/images/guiltyGear/PRIORITY1.png" | relative_url }})
![PRIORITY]({{ "/images/guiltyGear/PRIORITY2.png" | relative_url }})
![PRIORITY]({{ "/images/guiltyGear/PRIORITY3.png" | relative_url }})
![PRIORITY]({{ "/images/guiltyGear/PRIORITY4.png" | relative_url }})
![PRIORITY]({{ "/images/guiltyGear/PRIORITY5.png" | relative_url }})

Beware, your opponent can also activate PRIORITY to steal yours!
{% include video.html url="/videos/PrioritySwitch.mp4" %}
back-and-forth...
{% include video.html url="/videos/PrioritySwitchALot.mp4" %}

# Super Advances/Retreats
{% include video.html url="/videos/SuperAdvanceExample.mp4" %}

