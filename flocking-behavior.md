<h1 style="text-align: center">Building Particle Systems to Simulate Flocking Behaviors</h1>
<div style="text-align: center">Aaron George</div>

<div style="page-break-after: always; break-after: page;"></div>

___

_Abstract_. 

---



## Introduction



### 1.1 Background

​	Particle systems originated in 1982 as a solution for simulating phenomena like clouds, smoke, water, fire, and other *fuzzy* objects that could not be graphically modeled through the technology of the time (Reeves, 1983). Reeves, an animator at Lucasfilm Ltd., used the word *fuzzy* to describe objects that have dynamic and fluid structures. In other words, these objects are not rigid and their surface and shape is not always well defined. While working on the movie *Star Trek II: The Wrath of Khan*, Reeves spearheaded the development of a technique that could enable the procedural generation of fuzzy objects. Specifically, a scene in the movie required a wall of fire that ripples over a planet (Shiffman, 2012). To achieve the desired graphic, particle systems were implemented and used to imitate the features of fire.

​	The applications of particle systems is not confined to movies - they have been used in several settings like video games and digital animation to generate realistic simulations of natural phenomena. This discussion focuses on building and using a particle system to simulate flocking behavior in animals. This specifically pertains to birds, fish, and bees - species that are known to exhibit specific behaviors while traveling in groups. 



### 1.2 Processing

​	This discussion utilizes *Processing*, a community driven tool that can be used to teach the fundamentals of programming in a visual context. 	

>  Processing is a flexible software sketchbook and a language for learning how to code within the context of the visual arts. Since 2001, Processing has promoted software literacy within the visual arts and visual literacy within technology. There are tens of thousands of students, artists, designers, researchers, and hobbyists who use Processing for learning and prototyping.
>
> *— The Processing Foundation*

​	Keeping with the language of the visual arts, individual projects in Processing are typically referred to as *sketches*. Rendering of visuals occurs within a `draw` function. Accordingly the basic structure of a Processing sketch looks like this:

```java
void setup() {
    // called once at the beginning
}

void draw() {
    // called once every frame
}
```

​	Notice that a `setup` function is also included. This function is executed one time before the sketch is rendered and is typically used for initial tasks such as setting the canvas size, background color, specifying the renderer, initializing variables, and variable assignment. The `draw` function contains the code that is executed each frame and is typically used to draw images or visuals on the canvas. More details about Processing, the Processing Foundation, and the Processing Community can be found [here](https://processing.org/).



### 1.3 Vectors

​	An important construct used throughout this project is a *vector*. Simply stated, a vector is an entity that has both magnitude and direction (Shiffman, 2012). The magnitude of a vector determines its length, while the direction determines which way the vector is pointing. The idea of a vector is integral to this discussion because it has mathematical properties that can be utilized to transform points in Euclidean space to give the illusion of motion. Thankfully, Processing contains several structures and methods to allow the mathematical manipulation of vectors in two and three dimensions.



## Particles

### 2.1 Properties of Particles

### 2.2 Implementing the Particle System



## Flocking

### 3.1 Boids

### 3.2 The Flocking Algorithm

<div style="text-align: center;">
    <img src="images/no_flocking.gif">
</div>



<div style="text-align: center;">
    <img src="images/flocking.gif">
</div>

## Using the Flocking Algorithm

### 4.1 Avoiding Predators

<div style="text-align: center;">
    <img src="images/predator_no_avoidance.gif">
</div>



<div style="text-align: center;">
    <img src="images/predator_avoidance.gif">
</div>


### 4.2 Rorschach


<div style="text-align: center;">
    <img src="images/rorschach_0.gif">
    <img src="images/rorschach_1.gif">
    <img src="images/rorschach_2.gif">
</div>


## References

Reeves, 1983 https://www.lri.fr/~mbl/ENS/IG2/devoir2/files/docs/fuzzyParticles.pdf