---
layout: post
title:  Quaternions
description: The source of nightmares
date:   2024-12-31 01:01:00 +0000
image:  '/images/zombies/ZombiesBug8.jpg'
tags:   [Game-Development, BTS, Coding, Debugging,Zombies]
---
> I hate quaternions.
>
> <cite>me</cite>

![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug1.jpg" | relative_url }})
> I hate quaternions so much.
>
> <cite>me</cite>

![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug2.jpg" | relative_url }})
> With every fiber of my being.
>
> <cite>me</cite>

![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug5.jpg" | relative_url }})
> Seriously.
>
> <cite>me</cite>

In the world of 3D game development, quaternions are dark magic. They are mathematically dense, impossible to visualize intuitively, and yet, you have to use them.

Think of quaternions as a way to represent an orientation or a rotation in 3D space using four numbers: $(x, y, z, w)$. While they look intimidating, they solve the messy problems that traditional methods like Euler angles (pitch, yaw, roll) create.


# Why Use Quaternions for Bones?

In skeletal animation, a "skeleton" is a hierarchy of bones. If you rotate a shoulder, the elbow, wrist, and fingers must follow. This requires constant, smooth calculations.

In my case, I had to program ragdolls; in my 3D zombies game, zombies had to fall over when they died, because you know, realism. But, this was seemingly impossible to program. Every YouTube tutorial got it so wrong (maybe I'm the common denominator). I was hopeless.
I actually had to learn how to program... quaternions.
![Zombies Game Quaternion Bug]({{ "/images/zombies/3DModelBones.jpg" | relative_url }})

### 1. Goodbye, Gimbal Lock
Euler angles represent rotation as three successive turns. If two of those axes align (e.g., you rotate the pitch $90^\circ$ degrees), you lose a degree of freedom. Your bone suddenly "snaps" or refuses to move in a certain direction. Quaternions represent rotation as a single state in 4D space, making Gimbal lock mathematically impossible.

![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBoneEditor.jpg" | relative_url }})

### 2. Perfect Interpolation (SLERP)
> yall ever slerp your quaternions?
>
> <cite>someone</cite>

When a character moves from a "rest" pose to a "punch" pose, the engine has to calculate all the frames in between. 
* **Euler interpolation** often looks "robotic" or takes the long way around.
* **SLERP (Spherical Linear Interpolation)** allows quaternions to find the shortest, smoothest path between two rotations. This is why character joints look fluid rather than twitchy.



### 3. Memory and Performance
While a $3 \times 3$ rotation matrix uses 9 numbers, a quaternion uses only 4. When you’re calculating the transformations for a character model with 100+ bones at 60 frames per second, that memory and computational savings add up quickly.

---

## The Mathematical Anatomy
You don’t need to be a mathematician to use them (most engines like Unity or Unreal handle the heavy lifting), but it helps to know what’s under the hood. A quaternion is defined as:

$$q = w + xi + yj + zk$$

Where:
* **$w$** is the scalar part (related to the angle of rotation).
* **$x, y, z$** is the imaginary vector part (representing the axis of rotation).

If you want to rotate a bone by an angle $\theta$ around a unit axis vector $(u_x, u_y, u_z)$, the quaternion is:

$$q = \left[ \cos\left(\frac{\theta}{2}\right), u_x \sin\left(\frac{\theta}{2}\right), u_y \sin\left(\frac{\theta}{2}\right), u_z \sin\left(\frac{\theta}{2}\right) \right]$$

---

## Quaternions vs. Euler Angles

| Feature | Euler Angles (Pitch/Yaw/Roll) | Quaternions |
| :--- | :--- | :--- |
| **Human Readable** | Yes (e.g., "Rotate 90 degrees") | No (e.g., "0.707, 0, 0.707, 0") |
| **Gimbal Lock** | Yes | No |
| **Interpolation** | Jerky / Non-linear | Smooth (SLERP) |
| **Storage** | 3 Floats | 4 Floats |
| **Best Use Case** | Editor UI / Simple UI input | Skeletal Physics / Core Engine Math |

---

# Zombies Game
Where I applied quaternions:
<p><iframe src="https://www.youtube.com/embed/JoODb3_P6Z0?si=osUfGsc3tR-n_3H-" frameborder="0" allowfullscreen></iframe></p>

Here's a photo gallery of the hell I went through with quaternions!
![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug.jpg" | relative_url }})
![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug1.jpg" | relative_url }})
![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug2.jpg" | relative_url }})
![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug3.jpg" | relative_url }})
![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug4.jpg" | relative_url }})
![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug5.jpg" | relative_url }})
![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug6.jpg" | relative_url }})
![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug7.jpg" | relative_url }})
![Zombies Game Quaternion Bug]({{ "/images/zombies/ZombiesBug8.jpg" | relative_url }})
![Zombies Game Quaternion Bug]({{ "/images/zombies/3DModelBones.jpg" | relative_url }})
