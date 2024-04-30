# Challenges Faced
Many of the challenges that I faced personally were related to the fact that I was unfamiliar with Unreal at the start of the project. When problems took a large amount of time for me to complete, that was normally because I was getting used to the software, and the way it's organized (which was somewhat difficult, especially since the documentation is generally very plain). However, naturally there were some specific challenges which required more than familiarity with the software to solve.

## Aiming Issues
One of the most important features I worked on was Giovani's web shooting, which involves one of the players using the mouse to aim and shoot webs to navigate and complete platforming challenges. I looked up a tutorial to use as a start of how to aim, which involved the use of the Unreal `Convert Mouse Location to World Space` function and a line trace from the character to the mouse's position in order to do basic actor placement with the mouse. We opted to not use projectiles, but to spawn webs at the hit location of the trace with a delay to mimic a projectile without the extra work.

While the function does return an accurate position in the world, it doesn't consider visible boundaries; essentially, the position would be way further into the world than was necessary, which caused a very limited range of motion for the player's aim. Additionally, the range of motion was also dependent on the camera angle to the players; if the view was top-down, you basically couldn't aim at all. This led to a system that was only partially functional; in order to avoid problems when designing the level, the player would need to be able to aim anywhere on screen (with object interaction relative to the character).

Below is some of the original shooting code, along with a pre-alpha video demonstrating it (from week three of our blog): <br />
![shooting_functionality](https://github.com/aegross/imgd4000-portfolio/assets/48368165/f7a7087f-8d15-40bb-b7f8-34248188142e)

https://github.com/aegross/imgd4000-portfolio/assets/48368165/180ac090-72c0-42c6-bbaf-4fc09408abb7

In order to fix this problem, we realized that we needed to implement another line trace from the camera to the mouse position. <br />

![image](https://github.com/aegross/imgd4000-portfolio/assets/48368165/7178051b-9f25-4130-b428-41f6e84b58e7)

We did this so that we could triangulate the final hit position using both traces. First, a trace is made from the currently selected camera to the mouse's location. Then, the hit location from that trace is used as the end of a trace from the player. The hit location of the second trace is where the final web is spawned, in order to account for possible obstacles. This results in a web spawned at a location that makes sense from the player's point of view (the camera), while taking the world into consideration.

Below is the final blueprint code, along with a collection with most of the VFX I created and implemented, which includes shots of Giovani using the updated aiming system:
![image](https://github.com/aegross/imgd4000-portfolio/assets/48368165/e9d2688e-df09-4a79-b63f-bb50d5b2fe39)

https://github.com/aegross/imgd4000-portfolio/assets/48368165/957bc740-f42c-43f4-8acd-da305a9dbf27

_My recommendation for future 4000 students: The final product was a pretty good system for basic 3rd-person aiming- but it's not compatible with controllers. Feel free to steal it! If you want controller support, you'll have to take a different route. However, you may have more options available to you if you aren't making your own character controller from scratch like we did._

## Splines are Hard to Work With (Visually)
Another main component of implementing the shooting feature was implementing visual aspects of aiming to the player. I chose to do this by creating a straight line that connected the player to where shots would "land" (the hit location of the final trace described above). <br />
I was originally going to just implement a crosshair, but I got the idea to do a line from debugging; the debugged line trace with the hit location shown served as a much more helpful indication of where I was going to place webs than the crosshair I was using. Since I couldn't just make the debug line visible (for multiple reasons, including aesthetic ones), I needed to find a different way to implement the aiming line.

I decided to implement it using a spline, whose starting and ending points were the same as the the aiming trace's line. However, moving the spline and moving _visible_ spline meshes is two separate processes, which I didn't realize when I had started testing. In order to have good-looking spline meshes (that didn't just stretch a texture when the line was made longer), I made a single spline with two cylindrical meshes and put a material on top of them. (Using a material instead of a textured mesh meant that there was no visual distortion) Originally, I'd used one of the laser pointer materials from the starter kit, but Jay was able to make a better-looking dotted line material for it.

The most annoying part was figuring out how to create and effectively update the spline meshes. I referenced a tutorial for the creation of the meshes, but figured out the process for updating them on my own. The aim line was an actor placed in the scene, consisting of a spline and its' meshes, and connected to Giovani in the editor.

Spline mesh creation: <br />
![spline_construction](https://github.com/aegross/imgd4000-portfolio/assets/48368165/853c9e8e-d3b1-4f7b-8079-b568fc1ff782)

Updating the spline mesh (every tick): <br />
![spline_update](https://github.com/aegross/imgd4000-portfolio/assets/48368165/312db3b8-2ea3-4112-ab5e-83378f3bfc5f)

In order to be able to update the position of the meshes every tick, I needed to make sure there was a consistent number of spline points _and_ meshes. I could have made the number of meshes scale to the spline's length, which would have been good for textured meshes, but that didn't allow the math to work out for updating their locations. That's why I used materials instead. 

_My recommendation for future 4000 students: if you're going to make visible splines whose meshes will need to move and stretch, make sure the visuals are handled with a material that won't look bad if the mesh it's on scales and stretches a lot. You'll also need to have a conisistent number of meshes (or another system to properly handle an unknown amount). If you're making a visible spline that's static, make a spline with an adjustable number of meshes so that it looks good no matter what kind of material or texture you use._

_Splines are valuable tools, but the ones in Unreal are a bit wonky to work with, and take some getting used to._

## Grounding and Jumping
In order to prevent players from flying, grounding checks are crucial. Since we didn't use Unreal's character movement components (which was because we needed to support two players; we created our own character controller and input system), we didn't have pre-created grounding checks. This meant that we had to make our own.

Below is the character grounding check, used for both Giovani and Marcelo. Originally, this code was just written for Giovani, but it was refactored to be used by both characters, since Marcelo does have to "recharge" his flight ability by landing. Additionally, in order to animate jumping properly, a grounding check was necessary for both characters. <br />
![image](https://github.com/aegross/imgd4000-portfolio/assets/48368165/53f0ec60-a895-47a4-b89a-902f1579dc22)

A sphere trace is cast from the location of the center of the character's capsule to _slightly_ below the bottom of the character (due to the spheres' radii), and if there is a collision, the character is considered grounded. This was simple, but had to be tweaked multiple times due to the addition of a function to check if the character was in a web (`IsInWeb?`), along with boolean variables for animation (`IsGrounded?`) and audio (`CanFallSound`). We also had to make sure that the rope was excluded from the grounding check, since we didn't want the players to be able to jump off of the rope. Milo handled that part.

While having to move and refactor the blueprint code multiple times was annoying, it ended up being the best practice in the end, so it was worth it.

_My recommendation for future 4000 students: Don't build a character controller from scratch if you don't have to; the customizability is nice, but it takes up a good amount of development time. If you do, make sure that your "basic" playable character has all possible necessary features from the start! There's a lot of room for error- make sure to think of everything when debugging._

## Animation Blueprints
I'll put this plainly: Unreal's animation blueprints are both incredibly helpful **and** incredibly annoying. 

Since animation blueprints get applied to a skeletal mesh and _not_ an actor directly, the flow of information is unidirectional; you can provide information (such as boolean values) to an animation blueprint from an actor, but not the other way around, as far as I know (while this isn't really necessary, it would have been useful in a couple of cases). Animation blueprints also have different functionality (as expected), which can cause some minor problems.

This means that all information necessary for animating a character (knowing if they're grounded, knowing how fast they're moving, etc.) has to be either:
- passed from the actor that the skeletal mesh is assigned to
- determined by the code inside the blueprint itself, easiest when using Unreal's character movement components... which we didn't use.

This meant that extra changes needed to be made on the character blueprint side in order to pass information to the animation blueprints. This included whether or not the character was in the air (for both Marcelo and Giovani), and whether or not the player was aiming (for Giovani only). It was only possible to get the information from the characters by casting the animation's pawn owner to Character_Moth or Character_Spider _every tick_, which worked but wasn't ideal.

Marcelo's animation event graph: <br />
![image](https://github.com/aegross/imgd4000-portfolio/assets/48368165/cc0c9039-24b9-4717-906c-2c50388b7e60)

Giovani's animation event graph: <br />
![image](https://github.com/aegross/imgd4000-portfolio/assets/48368165/0ec9f08f-765d-49e4-80d9-40ec99ed1ad4)

Configuring the state machines used for determining what animations should play at what times was also more difficult than it should have been for me personally. I made the states too complex at first, with more states than were necessary, which caused the machines to sometimes get stuck in cases they couldn't get out of; this happened for jumping more than anything.

Using Unreal's Blendspaces to mix idle, walking, and running animations for the characters was very successful, compared to using states in a state machine.

Below is a video showcasing an early version of Marcelo's blendspace, which blends his walking and running animations: <br />
https://github.com/aegross/imgd4000-portfolio/assets/48368165/a33a0f73-bb4c-479f-b588-bdf0ff4b317c

_My recommendation for future 4000 students: utilize animation blueprints, but remember that you'll need to pass in information to them. So, if possible, keep track of all information you'd need to send over in variables while you're implementing features so you don't have to do it retroactively later. Additionally, use blendspaces! It was very easy to get the idle, walking, and running animations to flow together seamlessly. Lastly, for jumping, make sure the artists know whether or not they'll need to split it into a launch/land, or keep it all together. (This goes for other animations too; any where there is a starting action and an ending action)_
