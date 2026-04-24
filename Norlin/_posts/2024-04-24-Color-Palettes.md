---
layout: post
title:  Color Palettes
description: To change the color of your chosen character, I spent hours programming a shader and debugging...
date:   2026-04-24 00:00:00 +0000
image:  '/images/ShoShoOverUnder.jpg'
tags:   [Game-Development, BTS, Coding, Debugging]
---
In the character selection screen, you can select the color of your character:
![Color Selection]({{ "/images/CharacterSelectColors.jpg" | relative_url }})
![Color Selection]({{ "/images/FoilistShoColors.png" | relative_url }})

Wow, it even changes the color of effects:
![Sho funky colors]({{ "/images/ShoShoOverUnder.jpg" | relative_url }})

Unfortunately, this was difficult to program. Far too difficult. I'm sick of shaders.
Let's start simple: palettes. I exported the Foilist's palette in Aseprite and got:
![Palette]({{ "/images/palettes/Foilist Palette WIP.png" | relative_url }}){: .pixel-art width="100%"}
Wait, this isn't starting simple at all. Because, later on, I kept getting this:
![Bug]({{ "/images/FoilistShaderBug.png" | relative_url }})
Spoiler alert, but later on, the shader code will have to reiterate through each pixel of the palette. However, the above palette has two rows, so the colors on the bottom are discounted. And yet, they have to be replaced... by what? That's the bug; different sized palettes past the 16 color count break the shader. So, I manually altered each palette to get:
![Palette]({{ "/images/palettes/Foilist Palette.png" | relative_url }}){: .pixel-art width="50%"}
![Palette 1]({{ "/images/palettes/Foilist Palette 1.png" | relative_url }}){: .pixel-art width="50%"}
![Palette 2]({{ "/images/palettes/Foilist Palette 2.png" | relative_url }}){: .pixel-art width="50%"}
![Palette 3]({{ "/images/palettes/Foilist Palette 3.png" | relative_url }}){: .pixel-art width="50%"}
So, as long as the colors stay in one row, the shader scales proportionally and can be used for any character, regardless of the color count.
![Sho Palette]({{ "/images/palettes/Sho Palette.png" | relative_url }}){: .pixel-art width="100%"}
I don't remember Sho having that many shades of red... that's not good... anyways!
# The code
Two custom parameters are made: the original palette and the new palette. These can be edited in the engine, or preferably, set in code. For example,
``` py
func colorPalette(sprite,colorCount,firstPalette,secondPalette):
	sprite.material.set_shader_parameter("ColorCount", colorCount)
	sprite.material.set_shader_parameter("Palette", firstPalette)
	sprite.material.set_shader_parameter("NewPalette", secondPalette)
```
The next code snippit will seem insane. I know. It's okay, I'm not crazy, even though it only makes sense to me. Still, I want to include it *(especially without any explanation because it's funnier that way)*
``` py
colorPalette(
	get_node(chars[plr1SelectionCharacter]+"Left"),
	globalSelections.colorTypes[chars[plr1SelectionCharacter]][0],
	globalSelections.colorTypes[chars[plr1SelectionCharacter]][1],
	globalSelections.colorTypes[chars[plr1SelectionCharacter]][plr1SelectionColor+1]
	)

colorPalette(
	get_node(chars[plr2SelectionCharacter]+"Right"),
	globalSelections.colorTypes[chars[plr2SelectionCharacter]][0],
	globalSelections.colorTypes[chars[plr2SelectionCharacter]][1],
	globalSelections.colorTypes[chars[plr2SelectionCharacter]][plr2SelectionColor+1]
	)
```
*after editing it to look better, it should be clearer what's going on*


![Visual Code]({{ "/images/ColorPaletteShaderCode.jpg" | relative_url }})