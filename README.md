# Exercise-06d-3D-Particles
Exercise for MSCH-C220, 19 April 2021

A demonstration of this exercise is available at [https://youtu.be/ZSZsNlW18gU](https://youtu.be/ZSZsNlW18gU)

This exercise is a chance to play with Godot's 3D Particles. Also, you will have a chance to export a game as a finished product.

Fork this repository. When that process has completed, make sure that the top of the repository reads [your username]/Exercise-06d-3D-Particles. *Edit the LICENSE and replace BL-MSCH-C220-S21 with your full name.* Commit your changes.

Clone the repository to a Local Path on your computer.

Open Godot. Import the project.godot file and open the "FPS" project.

In res://Game.tscn, I have provided a starting place for the exercise: the scene contains a FPS character in an empty warehouse with five moving targets. Your gun can shoot those targets, but you will need to provide feedback to the player.

First, we need to create a scene for the explosion. In Scene->New Scene, Create Root Node: 3D Scene. Name the Spatial node "Explosion". Right-click and Add Child Node. Select AnimatedSprite3D. In the Inspector panel, AnimatedSprite3D->Frames->New Sprite Frames, and then edit the newly created SpriteFrames.

In the AnimationFrames panel, select the "Add Frames from a Sprite Sheet" button, and select res://Assets/Explosion.png. The grid is 8 x 8; select all the frames and press the "Add 64 Frame(s)" Button. Set Speed(FPS)=30 and Loop=Off. Return to the AnimatedSprite3D Inspector panel.

Set Flags->Billboard to Enabled, and Double Sided=Off.

Now, right-click on the Explosion node and Add Child Node. Select Particles. Select the new Particles node.

In the Inspector, set Particles->Amount: 200, Time->Lifetime: 0.75, Time->One Shot->On, Time->Randomness:1, Drawing->Local Coords: Off, Drawing->Draw Order: View Depth. Draw Passes->Pass 1: New Quad Mesh

Edit the New Quad Mesh: PrimitiveMesh->Material: New Spatial Material. 

Edit that Material: Flags->Transparent: On, Flags->Unshaded: On, Vetex Color->Use as Albedo: On, Parameters->Blend Mode: Add, Parameters->Billboard Mode: Particle Billboard, Particles Anim->H Frames: 6, Particles Anim->V Frames: 5, Particles Anim->Loop: On, Albedo->Texture: res://Assets/Smoke.png

Back in the Inspector for the Particles node, Process Material->Process Material: New Particles Material. Edit that material:

Trail->Divisor: 6, Emission Shape->Shape: Sphere, Emission Shape->Sphere Radius: 0.8, Direction->Direction: 0,1,0, Direction->Spread: 0, Gravity->Gravity: 0,0,0, Initial Velocity->Velocity: 5, Initial Velocity->Velocity Rand: 0.1, Angular Velocity->Velocity: 40, Angular Velocity->Velocity Rand: 1, Linear Accel->Accel: 4, Linear Accel->Accel Random: 1, Angle->Angle: 360, Angle->Angle Random: 1, Scale->Scale Random: 0.8, Scale->Scale Curve, New Curve Texture (edit it so it starts at 0, goes to 1 about a third of the way and then goes back to zero). Colr->Color Ramp: new Gradient Texture (edit it so it starts at #212529 and goes to #ced4da). Animation->Speed: 1, Animation->Offset: 1, Animation Offset Random: 1

Attach a script to the Explosion node, res://Explosion/Explosion.gd:

```
extends Spatial

func _ready():
	$AnimatedSprite3D.play()
	$Particles.emitting = true

func _physics_process(_delta):
	if not $AnimatedSprite3D.playing and not $Particles.emitting:
		queue_free()

```

Then save the scene as res://Explosion/Explosion.tscn

In res://Player/Player.gd, add the following after line 3:
```
onready var Explosion = load("res://Explosion/Explosion.tscn")
onready var Explosions = get_node("/root/Game/Explosions")
```

And then add the followin after (what is now) line 27:
```
				var explosion = Explosion.instance()
				Explosions.add_child(explosion)
				explosion.global_transform.origin = $Pivot/RayCast.get_collision_point()
```

Show the Laser pointer in /Game/Player/Laser, and test it out. Does this seem more satisfying?

When you are happy with the result, go to Project->Export. Tap Add… and then select Mac OSX. The default settings should work; export the project as project.dmg. If Godot tells you to download a template or other resources, follow those steps until you are able to export the file.

If you are not using a Mac, feel free to export other project types to test. 

Quit Godot. In GitHub desktop, add a summary message, commit your changes and push them back to GitHub. If you return to and refresh your GitHub repository page, you should now see your updated files with the time when they were changed.

Now edit the README.md file. When you have finished editing, commit your changes, and then turn in the URL of the main repository page (https://github.com/[username]/Exercise-06d-3D-Particles) on Canvas.

The final state of the file should be as follows (replacing the "Created by" information with your name):
```
# Exercise-06d-3D-Particles
Exercise for MSCH-C220, 19 April 2021

An exploration of Godot's 3D Particles and 3D Sprites.

## Implementation
Built using Godot 3.2.3
Exported for MacOS as project.dmg

The blaster and target models are from the [Blaster Kit](https://kenney.nl/assets/blaster-kit) at kenney.nl.

The skybox was downloaded from [HDRIhaven.com](https://hdrihaven.com/hdri/?c=indoor&h=empty_warehouse_01).

The smoke particle assets were downloaded from [opengameart.org](https://opengameart.org/sites/default/files/Smoke30Frames_0.png)

The explosion animation was also downloaded from [opengameart.org](https://opengameart.org/content/explosion-sheet)

## References
None

## Future Development
None

## Created by 
Jason Francis
```
