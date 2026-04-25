---
layout: post
title:  Gameplay Balancing
description: Transitioning fencing into a virtual form
date:   2026-04-24 00:00:00 +0000
image:  '/images/me/me_7.jpg'
tags:   [BTS]
---
How do you balance a fighting game that has no healthbar?

> This is my vision
>
> <cite>Daisuke Ishiwatari</cite>

# Blocking
Most of development, I resisted the mechanic of blocking. *Right of Way* should have NO blocking, because in fencing, if your opponent's blade comes at you, are you going to hold backwards and magically not get hit? Of course not! If you are undoubtedly going to get hit, guess what? That's your fault. You can't just hold down-back when the opponent lunges at your feet. Instead, ask yourself, "Why are my feet so close to his blade?"  
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

Blocking is punished by losing meter, and the attacker can cancel attacks into higher-priority attacks.

# Canceling
The order is as follows, from lowest to greatest:
1. Cut normal attack
2. Cut special attack
3. Lunge normal attack
4. Lunge special attack
For example, during a blockstring, you can cancel from a normal Cut into a Lunge, but once you do Lunge, you cannot cancel into a Cut.