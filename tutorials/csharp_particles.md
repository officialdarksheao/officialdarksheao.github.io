[Back](/)

# Basic Particle Engine
## C#, Windows Forms
This guide is to outline and document building a simple particle engine using C# in a windows form.

## Prerequisites
To follow along with my steps and screenshots - ensure you have visual studio 2019 installed.
At the time of writing I am using *Visual Studio Community 2019 - Version 16.10.4*
Note that screenshots will reflect using the dark theme and having the solution explorer moved to the left side of the IDE, matching the visual style as VSCode.

> Install Visual Studio Community at [Microsoft Visual Studio Downloads](https://visualstudio.microsoft.com/downloads/)

## Project Setup
Start up Visual Studio and choose to start a new project.

Choose C# and Desktop in the Language and Target filters, then choose the template for a windows forms app.
![Filters](/images/tutorials/csharp_particles/creating_new_project_filters.jpg)

Pick where you will save the source code of your project. For my example, I am naming it `ParticleEngine`. This will determine out initial namespace as well.
![Project name](/images/tutorials/csharp_particles/creating_new_project_naming.jpg)

Framework wise I am going with .net core 3.1 for overall compatibility.
![Framework selection](/images/tutorials/csharp_particles/creating_new_project_framework.jpg)

## Planning
A rough generalization of a particle system can be broken down into these parts:
* a Particle and it's properties
* * physical (tracking location, current momentum, weight, and viscosity)
* * visible (color or sprite)
* * time (life, deltas (or changes of weight, buoyancy, color, or momentum))
* A particle source (or emitter)
* * Location and momentum
* * Initial momentum vector of spawned particles
* * rate to spawn new particles
* Global Effects
* * Wind rate
* * Gravity Strength

Rendering these particles has to be determined. We want the Particle and Emitter systems to be generic enough that we could render them however we like, keeping them flexible and simple.
For the barebones system here, I am only going to render them in a windows form, in a canvas component.
"When" to render is something to think about. Windows forms have a caveat where the form itself can only be modified by the thread that created it.
We can leverage this by creating a timer and have the timer run at a 60 FPS rate. Every time it runs, we can loop over all our particle's current state (specifically visibility and location) to then draw them. This allows us to manage our particles from a thread independent to the rendering form.
I will be only doing the bare minimum of working with threads, but think of it as one set of processes that is figuring out the particle management, and another that only cares about where a particle currently is and what it should look like.

From this we can extrapolate the classes properties.

## Creating the particle classes
First I will add a new folder to put the classes in. Right click the project file (circle 1) and select new folder. I'm naming this one "Classes".
Each classes added in this folder will have .Classes added to it's namespace.

> Namespaces are a way to organize code. To access a class in a namespace different than the one it is defined in, add a `using {namespace};` to the beginning of the class you are needing to access it from.

![adding a folder to project root](/images/tutorials/csharp_particles/adding_new_folder_to_project_root.jpg)

Right click the newly created folder and choose Add New -> Class, creating a Particle and an Emitter class.

![added classes](/images/tutorials/csharp_particles/adding_classes.jpg)

From research and experience, I know I need a handful of the components in the Drawing class. Add a `using System.Drawing;` to the top of our two new classes.