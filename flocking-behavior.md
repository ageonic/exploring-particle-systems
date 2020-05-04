<h1 style="text-align: center">Building Particle Systems to Simulate Flocking Behaviors</h1>
<div style="text-align: center">Aaron George</div>

<div style="page-break-after: always; break-after: page;"></div>

___

_Abstract_. 

---



## Introduction



### 1.1 Background

Particle systems originated in 1982 as a solution for simulating phenomena like clouds, smoke, water, fire, and other *fuzzy* objects that could not be graphically modeled through the technology of the time (Reeves, 1983). Reeves, an animator at Lucasfilm Ltd., used the word *fuzzy* to describe objects that have dynamic and fluid structures. In other words, these objects are not rigid and their surface and shape is not always well defined. While working on the movie *Star Trek II: The Wrath of Khan*, Reeves described a technique that enables the procedural generation of fuzzy objects. Specifically, a scene in the movie required a wall of fire that ripples over a planet (Shiffman, 2012). To achieve the desired graphic, particle systems were implemented and used to imitate the features of fire.

The applications of particle systems is not confined to movies - they have been used in several settings like video games and digital animation to generate realistic simulations of natural phenomena. This discussion focuses on building and using a particle system to simulate flocking behavior in animals. This specifically pertains to birds, fish, and bees - species that are known to exhibit specific behaviors while traveling in groups. 



### 1.2 Processing

This discussion utilizes *Processing*, a community driven tool that can be used to teach the fundamentals of programming using Java in a visual context.

>  Processing is a flexible software sketchbook and a language for learning how to code within the context of the visual arts. Since 2001, Processing has promoted software literacy within the visual arts and visual literacy within technology. There are tens of thousands of students, artists, designers, researchers, and hobbyists who use Processing for learning and prototyping.
>
> *— The Processing Foundation*

Keeping with the language of the visual arts, individual projects in Processing are typically referred to as *sketches*. Rendering of visuals occurs within a `draw` function. Accordingly the basic structure of a Processing sketch looks like this:

```java
void setup() {
    // called once at the beginning
}

void draw() {
    // called once every frame
}
```

Notice that a `setup` function is also included. This function is executed one time before the sketch is rendered and is typically used for initial tasks such as setting the canvas size, background color, specifying the renderer, initializing variables, and variable assignment. The `draw` function contains the code that is executed each frame and is typically used to draw images or visuals on the canvas. More details about Processing, the Processing Foundation, and the Processing Community can be found [here](https://processing.org/).



### 1.3 Vectors

An important construct used throughout this project is a *vector*. Simply stated, a vector is an entity that has both magnitude and direction (Shiffman, 2012). The magnitude of a vector determines its length, while the direction determines which way the vector is pointing. The idea of a vector is integral to this discussion because it has mathematical properties that can be utilized to transform points in Euclidean space to give the illusion of motion. Thankfully, Processing contains several structures and methods that allow the mathematical manipulation of vectors in two and three dimensions.



## Particles



### 2.1 Properties of Particles

### 

Reeves (1983)  delineates three ways in which objects created with particle systems differ from objects created using traditional techniques. 

1. *Structure.* With particles systems, objects are not generated as a synthesis of well defined shapes that define the object's boundary. Instead, they are generated as *clouds* of primitive particles that define the object's volume.
2. *Lifetime.* Particle systems are not static. They comprise of individual particles that are created and destroyed as necessary.
3. *Shape.* Objects created with particle systems are not deterministic. This means that their shape and form are not completely defined. Instead, the shape and form is created and shaped through other pseudo random algorithmic techniques.

Note that the density of an object is determined by the number of particles in the system - greater density is achieved by increasing the number of particles. Unsurprisingly, at the core of a particle system is the individual particle - an entity that possesses several attributes.

Reeves describe seven properties that characterize a particle. Accordingly, a particle has a (1) position, (2) velocity, (3) size, (4) color, (5) transparency, (6) shape, and (7) lifetime. On creation of a new particle, an initial value must be determined for each of these properties. These properties control the structure, shape, density, and form of a particle system. The initial properties for each particle can be manipulated to enhance the dynamic qualities of the object.



### 2.2 Implementing the Particle System

The previous section discussed the characteristics of a particle. In this section, we will attempt to implement a particle object and render it through a Processing sketch.

First, we define the `Particle` class that will be the basis for all future sketches in this discussion.

```java
class Particle {
    PVector position, velocity, acceleration;
    float mass, size, speed, r, g, b;

    public Particle(float x, float y, float mass_, float size_, float speed_, float r_, float g_, float b_) {
        position = new PVector(x, y);
        velocity = PVector.random2D();
        acceleration = new PVector();
        mass = mass_;
        size = size_;
        speed = speed_;
        r = r_;
    	g = g_;
    	b = b_;
    }
}
```

Observe that not all of the properties described in the previous section are being defined. We only specify the attributes that are relevant to this discussion. The constructor allows the particle to be initialized with a specified position, mass, size, and maximum speed. The velocity of each particle is set to a random unit vector by default. Next, we can implement a method that renders the particle in our sketch.

```java
class Particle {
	/* ... */

    public void render() {
        push();
        stroke(r, g, b);
        fill(r, g, b);
        ellipse(position.x, position.y, size, size);
        pop();
    }
}
```

Finally, we implement the methods that simulate the *physics* of the particle. Note that these are all members of the `Particle` class, but will be discussed separately for clarity.

The `applyForce` method, quite simply, applies a specified force to the particle. Using Newton's Laws of Motion, we know that $F = m * a$, where $F$ is the force, $m$ is the mass of the object, and $a$ is the acceleration. From this equation, we have $a = F / m$. Accordingly, this method divides the given force by the mass of the particle, and adds it to the acceleration of the particle. This has been implemented in a way that allows several forces to be simultaneously applied to the particle. The acceleration of the particle can then be used to update its position.

```java
public void applyForce(PVector force) {
    this.acceleration.add(force.div(mass));
}
```

The `update` method manipulates the position of the particle. We begin by adding the current acceleration to the velocity, and subsequently adding the velocity to the current position. Note that  `velocity` stores the current direction of motion for a particle. This process  shifts the particle's position through each frame, giving the illusion that the particle is moving based on the amount of force applied to it.

```java
public void update() {
    velocity.add(acceleration);
    position.add(velocity);
    velocity.limit(speed);
    acceleration.mult(0);
}
```

The following methods allow a certain degree of control over the particle. `wrapEdges` allows a particle to move freely on the canvas by making it seems as though the edges are connected to each other. `repelEdges` changes the velocity of a particle to the opposite direction as soon as it touches an edge. `constrain` is a similar method that restricts the particle's movement to specified limits.

```java
public void wrapEdges() {
    if (position.x < 0) position.x = width;
    if (position.x > width) position.x = 0;
    if (position.y < 0) position.y = height;
    if (position.y > height) position.y = 0;
}

public void repelEdges() {
    if (this.position.x <= 0 || this.position.x >= width) this.velocity.x *= -1;
    if (this.position.y <= 0 || this.position.y >= height) this.velocity.y *= -1;
}

public void constrain(float u, float d, float l, float r) {
    if (this.position.x <= l || this.position.x >= r) this.velocity.x *= -4;
    if (this.position.y <= u || this.position.y >= d) this.velocity.y *= -4;
}
```

Finally, we can set up our sketch to witness a particle in action. 

```java
Particle p;

void setup() {
  size(300, 200);
  p = new Particle(width/2, height/2, 1, 5, 4, 255, 255, 255);
}

void draw() {
  background(55);
  p.applyForce(new PVector(0, 0.2)); 
  p.update();
  p.wrapEdges();
  p.render();
}
```

By replacing `line 14` with   `p.repelEdges();` and `p.constrain(50, 150, 100, 200);` respectively, we receive the following sketches.

<div style="text-align: center;">
    <div style="display: inline-block;">
        <img style="padding: 4px;" src="images/particle_wrap.gif">
        <div>
            Particle Wrapping Edges
        </div>
    </div>
    <div style="display: inline-block;">
        <img style="padding: 4px;" src="images/particle_repel.gif">
        <div>
            Particle Repelling Edges
        </div>
    </div>
    <div style="display: inline-block;">
        <img style="padding: 4px;" src="images/particle_constrain.gif">
        <div>
            Constrained Particle
        </div>
    </div>
</div>

We now have a basic Particle object that we can extend to implement the flocking algorithm.



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