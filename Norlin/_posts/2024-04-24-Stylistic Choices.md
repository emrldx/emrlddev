---
layout: post
title:  Stylistic Choices
description: Determining the aesthetics of Right of Way
date:   2026-04-10 01:00:00 +0000
image:  '/images/MusashiSuper.png'
tags:   [Game-Development, BTS]
---
How were the aesthetics of Right of Way shaped over time?

![Art]({{ "/images/marketing/LogoTrimmed.png" | relative_url }})

# Indicating right of way
Here are all the indicators at once--the overhead arrow, visible bar of meter, and body trails:
{% include video.html url="/videos/trails/BodyTrails.mp4" %}

### Why so many "distractions"?
While the **right of way** arrow at the top was a great addition, I do not expect players to look *over* the combat and watch the arrow slowly transition. The player *should* focus on the fencing in-front of them--but, if they do not understand the **right of way** rule fully, they still need a visual guide. Which became the body trail.

## Body Trails
![Right of Way]({{ "/images/trails/BodyTrails2 0.jpg" | relative_url }})

As the player moves, duplicates of their sprite follow. Well, only the player possessing **right of way** has this trail; this acts as a visual indicator of who has **priority**.
{% include video.html url="/videos/trails/BodyTrails1.mp4" %}

<hr/>
# Blade Trail
![Right of Way]({{ "/images/marketing/library_hero.png" | relative_url }})
As the Foilist’s blade moves, a trail of red/green (depending on being player 1 or player 2) follows the tip. This improves tracking, helping players comprehend the movement of the blade and whether attacks landed; large or multi-hitting attacks may be poorly telegraphed and/or explosive, so the trail guides the viewer’s interpretation.

#### Blade Trail vs Body Trail
The trail system from before provided much of the logic for the body trail script. However, the key differences are:

| Blade Trail | Body Trail |
| :--- | :--- |
| Both players | Whoever has **right of way** |
| Instantaneous | Delayed |
| Follows the tip of the blade | Follows the entire sprite (even as it animates) |
| Detects the best position via an algorithm | Detects appearance via the character Sprite2D's variables |
| Color changes over a gradient | Color is hard-set (but blends into transparency) |
| Many invisible points | Many faded sprites |


<hr>
# Pixel Art
### Street Fighter 3
SF3 inspires the art and gameplay of Right of Way. Unforutnately, it is near-impossible for me to replicate SF3's beautiful sprite work. Even so, many of my animations pay homage to the fighting game I grew up playing.
Some of the beautiful art from Street Fighter 3:
![Ken]({{ "/gifs/(kensa3).gif" | relative_url }})
![Ken]({{ "/gifs/(kenbmk).gif" | relative_url }})
![Ken]({{ "/gifs/(kenfhk).gif" | relative_url }})
![Ken]({{ "/gifs/(kenhk).gif" | relative_url }})
![Ken]({{ "/gifs/(kenhmk).gif" | relative_url }})
![Ken]({{ "/gifs/(kensrk).gif" | relative_url }})
![Ryu]({{ "/gifs/(ryusrk).gif" | relative_url }})
![Ryu]({{ "/gifs/175px-(ryuatsk).gif" | relative_url }})
![Ryu]({{ "/gifs/175px-(ryuhdk).gif" | relative_url }})
![Ryu]({{ "/gifs/175px-(ryujsg).gif" | relative_url }})
![Ryu]({{ "/gifs/175px-(ryucmk).gif" | relative_url }})
![Akuma Raging Demon]({{ "/gifs/akumaRagingDemon.gif" | relative_url }})

Makoto's sprite work stands out in a game of gorgeous art. She feels amazing to play--despite her unorthodox control scheme--due to how her animations flow into one another. Some examples:
![Makoto]({{ "/gifs/(makotochp).gif" | relative_url }})
![Makoto]({{ "/gifs/(makotofkage).gif" | relative_url }})
![Makoto]({{ "/gifs/(makotolk).gif" | relative_url }})
![Makoto]({{ "/gifs/175px-(makotocmk).gif" | relative_url }})
![Makoto]({{ "/gifs/175px-(makotochk).gif" | relative_url }})
![Makoto]({{ "/gifs/175px-(makotofhk).gif" | relative_url }})
![Makoto]({{ "/gifs/175px-(makotohayate).gif" | relative_url }})
![Makoto]({{ "/gifs/175px-(makotomk).gif" | relative_url }})
The smears, stretches, pauses... wow.
![Makoto]({{ "/gifs/175px-(makotosa3).gif" | relative_url }})

<hr>
# The Samurai Movie Influence
*Right of Way* began as a fencing simulator, but it also evolved into a tribute to 1950s and 60s Japanese cinema. 

### **The Kurosawa Aesthetic**
References to films like *Seven Samurai*, *Yojimbo*, and *Sanjuro* permeate the game. The "one-hit-to-win" mechanic isn't just a gameplay choice; it’s an attempt to capture the "one-hit kill" tension found in the climactic duels of Toshiro Mifune’s filmography.

### **Bridging Fencing and Bushido**
As a fencer and a fan of samurai media, the developer noted the striking similarities between the two:
1.  **The Touch:** In fencing, a single touch scores.
2.  **The Mistake:** In a samurai duel, one wrong step is fatal.

This synergy creates a fighting game where the emphasis is not on practicing 50-hit combos in training mode, but on those quiet, intense moments where both players are within reach, and the first person to flinch loses everything. This appeals most to lovers of "footsies" and playing neutral in fighting games.



# Visuals
Right of Way needed a visual upgrade.
### Trails
One of the new visuals is the trails tracking algorithm.
![Trails]({{ "/images/Trails.png" | relative_url }})
As the Foilist’s blade moves, a trail of red/green (depending on being player 1 or player 2) follows the tip. This improves tracking, helping players comprehend the movement of the blade and whether attacks landed; large or multi-hitting attacks may be poorly telegraphed and/or explosive, so the trail guides the viewer’s interpretation.



![Background]({{ "/images/BackgroundBannerCropped.png" | relative_url }})
![Sho Boxes]({{ "/images/ShoBoxesPurple.png" | relative_url }})
![Sho Zoomed]({{ "/images/ShoShoZoomed.png" | relative_url }})
![Musashi Clash]({{ "/images/MusashiClash.jpg" | relative_url }})