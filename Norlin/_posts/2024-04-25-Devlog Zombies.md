---
layout: post
title:  Zombies 3D Devlog
description: Behind-the-scenes, including the script
date:   2024-12-31 00:01:00 +0000
image:  '/images/thumbnails/Zombies.jpg'
tags:   [Script,3D]
---
After writing a script, about 20% is deleted. Then, after recording voiceovers, about 30% is deleted. Trust me, videos are really trimmed down--the following script isn't nearly as much as I wrote, and definitely nowhere close to how much work I put in.

{: .important }
The following is copied-and-pasted from the huge devlog text file. Italics designate things to edit/show on screen.

3141 words.

I grew up on Call of Duty Zombies. Okay, well, I haven’t “grown up”. I’m still growing up on Call of Duty Zombies. If you also grew up knifing TED the bus driver and fearing the Panzer siren like I did, then you were also disappointed when Vanguard Zombies came out.
If you have no idea what I’m even talking about, *bro I’m here for gamedev stuff wtf is a Panzer siren* imagine loving the movie Joker for all its darkness and grit, waiting 5 years for a sequel, just for it to be a… musical.
Ok, pay attention: old Call of Duty good, new Call of Duty bad. *yknow, what everyone says before they drop $80 on the same game every 2 years.* I was worried that Call of Duty Black Ops 6 Zombies would be a let down. I mean, cmon, every Zombies game is less and less about what the Zombies community wants.
But, I’m a part of that community, and I can code somewhat well. So, I set out to create a 3D Round-based Zombies game that has what players actually want.
Or, let’s be honest–what I want. I love movement in shooter games. Yall ever played Banana Shooter? Yeah, didn’t think so. You know at least Team Fortress 2, right?? Ultrakill??
Nothing is more fun than moving around in creative ways, such as rocket jumping or shotgun jumping. In COD Zombies, the most fun moments are when a whole horde is on you, and all you have is the sprint button. I don’t believing in catwalk camping–Zombies is fun when you actively outmaneuver the zombies.
So, my game, emrld Opps Zombies, will focus on movement.
However, I am no triple-A game studio. I am just some bored kid who can draw pixel art and code fighting games. I would have to learn how to even create a 3D game.
Good thing random YouTube videos exist.
*ride bullets*
Yeah… I have to make this clear right now: Godot is the game engine I use. It is not as popular as, let’s say, Unity, and is intended to be used for 2D games–not 3D. So, the videos and documentation online are NOT that good.
Anyways, here are some bouncy balls. And a cube that should NOT be moving like that.
One video was really useful–here are some bean zombies.
If I can’t even get gravity to work, how am I going to make this???
But whatever, here’s a big bean boss.

I downloaded Blender and got to modeling a pistol. I didn’t even watch any videos, I just kinda clicked stuff and it eventually worked. When you use gamedev software enough, it becomes intuitive. Like this zombie model I made–just move some vertices around, open the command console and search for stuff, oh great now the right thumb moves the left arm.
Rigging the mesh gave me so much trouble that I won’t even talk about it. If you want all the extra details, I have an Instagram and TikTok, maybe look there. Maybe even follow. Great plug, right?

I had NO IDEA how to make ragdolls. When I killed a zombie, it just T-posed and rotated.
After spending a long time trying to figure out how to texture paint, I was able to make the zombies green–yknow, zombies are green or something.
But, like, they aren’t only green, I think. I decided to give them brown shirts and blue jean shorts. Wait, jean shorts, so jorts? *zoom out* Did I seriously give my zombie models jorts?
Anyways, I had to figure out how to make my jorts-wearing zombies ragdoll. *dani* Movement and ragdolls together make the best kind of game!
Basically, when the zombies animate, the mesh follows the armature bones. When the zombies die and stop animating, the armature bones have to follow the physical bones. Well I could make the physical bones fall, but the mesh… *zombie gets flung away*
This required some really difficult code. Something to do with “quaternions”. *dani* It’s alright, I’m the best teenage coder in the Northern Hemisphere, I’ve got this–*zombie compresses in on itself*
I’m cooked.
It’s ok, I can make things right–*large meshes*
It’s so over…
Here’s what I’m going for…
*drunken wrestlers, tf2, banana shooter ragdolls*
Here’s my game…

Ragdolls drove me insane.
*confusing snippets of my yapping*
It’s not my fault that there are like 2 videos that somewhat help making ragdolls, the documentation is confusing, quaternions are confusing, and PhysicalBoneSimulator3D is new in so I have to figure it out myself.

The whole time, I thought the skeleton would do stuff when I told it to do stuff. But, stuff was not doing. So, I set the skeleton to always be active instead of setting it active when the zombie needs to ragdoll. One small change and…

https://github.com/godotengine/godot/issues/18443
![[Pasted image 20241215122135.png]]

![[Pasted image 20240930090824.png]]
*talking to a brick wall over me yapping bout global pose*

I added rounds and zombie spawn rates. Aren’t the ragdolls so beautiful? They’re just so silly and goofy, right? *zooming in on round increasing*
I love ragdolls.

I finally added sounds to my game. I know you’re wondering: “oh glorious emrld whom I am subscribed to, how did you manage to make such sound effects for your 3D round-based zombies game that will win Game of the Year 2025?”
Well, my dedicated viewer, I used… nevermind, I cannot tell you. It is a very criticized practice in the game development and art community. *me typing in Elevenlabs: pls give me a gunshot sound pls im begging you please my game needs it desperately i have no idea how to make sound effects on my own*
*sfx plays and it’s “boy shut the hell up” some kind of meme sound*
The killmarker sound effect sounds disgusting and atrocious, but that’s ok, because I only had to pay $5. :) *whacking metal sheets video*

The player is still a bean in a world of humanoids. Here’s a player model I made, with the head looking way too round *I discovered the bevel button!!!*
The model doesn’t look up and down, so it’s pretty difficult to aim in third person. 
Oh, yeah, I’m adding third person to my game because I think it’d be cool. It is 100% a coincidence that Black Ops 6 Zombies also has third person. Seriously.
This isn’t even a joke. I planned out third person, swords, and a dedicated weapon slot *show Obsidian* a whole month before Black Ops 6 even came out. If anything, the triple-A gamedev studio is copying me. This doesn’t feel fair. Why can a large company steal from a little kid who only has Godot and a dream? Guys, boycott Treyarch and buy my game instead– *laughing away from mic as player model tweaks out at floor*

*laughing, playful mood* guys, what is this???? Why is my player model tweaking out??? What the hell is a quaternion??? *dani* Why does the mesh rotates 90 degrees when you fire??? I just want the player to be able to look up and down!!! I added two whole bones to the skeleton armature so you bend up and down, then bend left and right! But no!!! That means you rotate for no reason when you fire a gun!!!
Walking? Youre fine! Nothing happens! Reloading? You’re fine! Nothing happens because why would you twist around to reload??
It’s ok, I can fix this, I can make the bone rotate to a quaternion or something! *upside down* WHYYYYYY??? WHAT IS A QUATERNION???? *math quaternion explained over funny footage*
oh, cool, deleting something in the RESET animation fixed it. Why? I don’t know. Well, now I can’t rotate the upper body in animations, maybe there’s a problem with the bones in Blender. I’ll just rotate the upper body bone…
*legs upside down* WHAAAAAAT???
*more quaternion stuff*
Look, you can now run in different directions. Surely there are no unintended consequences–*tall torso… smashing keyboard video*

Enough headaches. Let’s make another gun! If you’ve played COD Zombies, you probably know of the M14 and Olympia. More importantly, how they’re both 500 points wallguns *so, bad* and you devote yourself to one. New Zombies games no longer have these guns and therefore the jokes have sadly died. But, not if emrld Opps Zombies has anything to say… Personally, I’m part of the M14 gang. You could say that the Olympia gang are emrld’s opps… zombies. *boo*

Anyways, all the green lines here are attempts to make the player model rotate. Remember quaternions from like a whole minute ago? Yeah, I have those under control now–*spinning*
Ok, I can finally slerp my bone quaternions to end quaternions. Yes, “slerp” is the correct word. If you have ever played a 3D game, you are guilty of slerping a man’s bone at least once. *spine and neck bones, to be exact. Richtofen sus comment*

Now that I can slerp like a professional, it’s time to move up in the world. I began making a map for my game. I used what’s called a heightmap to designate the height of the terrain at certain points, corresponding with the darkness at that point on an image. Basically, gray is high *hills*, white is low. *valleys*
I honestly had no idea what map to make, so I took inspiration from the samurai movie Harakiri. If you watched my first devlog on this channel, you would know I love samurai movies. The map is called “Ronin’s Rest”, and the goal is to have tight-knit bamboo areas then large areas for cinematic fights. The large areas make it easy to train zombies, so in them, boss zombies will spawn that the player can duel just like a samurai.
I added fog and, of course, I had to model the bamboo and everything myself. Such as… a shotgun. Yeah, that’s right, you thought I would only add the M14? How, then, could players make fun of Olympia users? What’s a movement-based shooter without shotgun jumping?
I also modeled tombstones to resemble the opening shots of a duel in Harakiri.

Yeah, figuring out door buys was difficult. The documentation on navigation is especially sparse. I spent half the time just figuring out which code to copy and paste.-
Think about it; in Kino der Toten, everyone sticks to their favorite side, right? I always go to the right and never buy the left-side door. Well, do zombies ever spawn ion the left-side area? I don’t know, I’ve never been there, but I assume not. That’s because they can only navigate–therefore, spawn in–in areas behind bought doors. The navigation area stays small and procedurally grows as players buy doors. I bet you’ve never thought about how they managed to pull that off.
*Developers could always just create loads of navigation areas and choose one every time a door is bought. But, cmon, who is making hundreds of navigation regions for every single possible door buy?
They could have a doors be obstacles (NavigationObstacle3D) that block navigation, and when doors are bought, the obstacles are deleted. But, if they block navigation, that means there’s a hole in the navigation area, therefore the entire navigation region has to be reset each time a door is bought. So, no matter what, they eventually have to reparse the geometry and rebake navigation.*
Long story short, I figured out how to bake navigation in runtime on an asynchronous thread without any lag. Even if the geometry takes a second to parse once a door is bought, that’s handled separately and the only consequence is that the player can’t run through the door for half a second until the navigation is baked.
Believe it or not, it was somehow easier to figure out all that with SCARSE documentation than it was to make the player model look up and down! I’m really learning…

Next, you gotta be able to throw weapons. That’s just funny or something, I dont know. Also, crouching and sliding are probably extremely important, just a guess. Of course, quaternions became a problem again, but this time I used my hard-earned wisdom and overcame them to create some amazing animations… *spam sliding*

What’s a samurai map without a samurai sword? I modeled a katana by using my own katanas as a reference, cause I’m just so cool like that.
M14, Olympia, Katana, what else does this game need? Well, wonder weapons can wait until the next video; this game needs a sniper. I modeled an AWP, which actually is not as original as you may think. The AWP is actually known in underground gaming circles because of this underrated game it’s in. You probably haven’t heard of it–it’s called CSGO. Yet another game that steals from emrld Opps Zombies.
I drew a scope image and modeled a scope for the AWP. You can scope in on zombies and hit epic trickshots now! I expect an edit of my game within 2 weeks time. You have 2 weeks to use the clips from this video to create a Modern Warfare 2 type edit. I will post your edit everywhere imaginable (such as my instagram and tiktok and discord and stuff) because it’d really funny. Seriously. That’d be hilarious.
For my finishing touches, I touched up the player model and finally improved the zombie model. As you can see, no bugs ever occur anymore in my amazing game.
Also, you can finally buy weapons around the map. No wallguns because I’m super original, obviously.

With all this, I have successfully safeguarded against a bad Black Ops 6 Zombies. Next video, I’ll add multiplayer, way more weapons, wonder weapons, bosses, and greatly improve the map as it’s very bare right now. I should be on track to beat the release of Black Ops 6–*looks at time while editing*
*sighs* damn quaternions. *fade to black*

One last thing: I 3D printed the player model. I had no reason to. It’s just really cool to feel my own work under my own fingertips. It gives a lot of motivation to work on my game as I can finally reap the rewards. Also, I 3d printed stuff from my 2D fencing fighting game, which I’ve talked about on this channel. Go check those devlogs out–they go way more in depth than I did for this video.
I have a Patreon, so if you love COD Zombies, love emrld, or love the future Game of the Year 2025 winner, then maybe support emrld and his hit games emrld Opps Zombies and Right of Way. Or, just go follow his Instagram and TikTok. At least join the Discord. Your choice. I’m just his spokesperson.
I’m not even emrld. I’m emrld’s consciousness uploaded onto a computer to manufacture Game of the Year winners and devlogs. Please help me. I visualize quaternions in my dreams. I can’t escape them.





Then, I animated the player model within Godot since the animations had to include the gun’s animations, SFX, code functions, etcetera. But, I learned why people say to just stick to 2D when using Godot.
It took me an hour to figure out how to even animate the bones. Then, every time I got it to work, something would break. This actually broke… me. I’d spend an hour or entire day fixing some stupid bug *character actually tweaking tf out*, get to animate for 5 minutes, then test the g ame to see the character spinning at mach 30! It made no sense why!
The rotation of the UpperBody bone would be fine one animation, and not keyframed on the next. So, its rotation should stay the same, right? NO! Go to the next animation, and the bone would rotate for no reason!!
It had something to do with quaternions. I didn’t know what quaternions were. While writing my own code to rotate the bones according to where the player looked, ![[bone rotation.png]]
I had to make quaternions from degree vectors, because that’s what Godot requires when you want to rotate a bone. Ok, so what’s a quaternion?
https://en.wikipedia.org/wiki/Quaternion
“In modern terms, quaternions form a four-dimensional associative normed division algebra over the real numbers, and therefore a ring, also a division ring and a domain.”
…what?

To be honest with you, I had a lot of homework/was busy around that time. I barely got to work on the game for more than 30 minutes at a time, and most of that time was within school as my teachers yapped. So, to have my game break in a new way every single time I fixed it, then finally thinking I understand the underlying problem, just to be hit with that definition? I actually wanted to delete the entire project and go back to what I'm already good at–making my 2D game, Right of Way.
But, then I remembered how trash Vanguard Zombies was… I had to push on just in case Black Ops 6 was also disappointing.

# Quaternions
https://www.youtube.com/watch?v=Ri2xIhcii8I

In the RESET animation, the rotation of the UpperBody bone was keyframed so that the model could look forwards. Without that keyframe, my model kept breaking and spinning in inhumane ways. But, after learning about quaternions and improving my code, I no longer needed the 3D Rotation keyframe in the RESET animation. I deleted it, and now the problem is solved. However, this means that I can never rotate the UpperBody bone (a very important bone) in an animation, or else the character will rotate weirdly.


roll of the bones in blender



11/3
just delete the global to make it rotate smoothly
I finally understand quaternions and can code them easily!!

11/4
delete cube first like always
based on harakiri duel location
“ronin’s rest”
had to learn what a height map was and made the map in blender
got navigation problems, zombies respawn to closest spawner if they are either too far away or cannot physically navigate to the player

bamboo
*spawn torus, ptsd from blender donut tutorial which I never even did*



# navigation region

was very difficult
reparsing the geometry and rebaking would cause freezing, but worked
so I tried to add polygons to the navmesh by giving it vertices
but the documentation is spare and sucks so I had no idea what I was doing
it didnt work
so finally after 1.5 days I decided to change the navmesh baking itself
now, it bakes depending on collisionshapes, meaning it does much less calculation. and, i changed it to monotone, so it bakes faster but with less accuracy.
the navmesh is slightly worse but hey at least you can buy doors now!
godot please add to your navigation documentation
