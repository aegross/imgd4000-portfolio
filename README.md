# imgd4000-portfolio
A collection of all the work I completed on *Sleeping with the Roaches*, my group's final project for IMGD 4000 at WPI in D24. 

This includes a detailed description of my role in the project, all the features I worked on, and architectural diagrams describing them (according to the requirements listed here: https://github.com/charlieroberts/imgd4000-2024/blob/main/development_portfolio.md). 

## What is *Sleeping with the Roaches*?
Sleeping with the roaches is a co-op platformer made using Unreal Engine 5.2 for IMGD 4000 (Technical Game Development II) at WPI. This was a group project created by myself and five others (the full team is listed below). 

While I was not able to contribute to the project as much as I had hoped to (due to IQP work, mostly), I'm still proud of the work I did, considering that my only general purpose engine experience before this was making one game for a class in Unity.

Our team website, which includes downloads and a weekly blog, is available here: https://stinkymilo.github.io/TeamWebsite/ <br />
The repository for the project is available here: https://github.com/StinkyMilo/NotABug/ <br />

### The Team
**Art**: Keenan Jones, Hayden Padula <br />
**Tech**: Audrey Gross, Justin Ignatowski, Milo Jacobs, Owen Knizak <br />

## What was my role in the project?

As listed in the game's credits, I mainly worked on:
- Spider & Web Programming (for Giovani, the playable spider character)
- Animation Programming (taking the animations made by the art team and implementing them with state machines)
- VFX (most done by me, the rest by Jay)

I also supplied some of the audio assets we planned to use (for footsteps), which I had initially recorded for a foley assignment when I took IMGD 2030 (Game Audio I).

I decided to tackle the bulk of the animation implementation and VFX work since I have artistic experience, but mostly because it seemed interesting. I had initially wanted to do some of the more mathematics-heavy work (such as aspects of the rope, which Milo focused on), but realized that other members of the team were more available and more interested in that work.

For Giovani, I implemented his grounding checks, the web shooting mechanic (including the spline line shown to the player), and most of the basic web behaviors. 
