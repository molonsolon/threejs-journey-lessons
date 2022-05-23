# Raycaster
A raycaster can cast a ray in a specific direction and test what objects intersect with it. 

Usage examples:
* detect if there is a wall in front of the player
* test if a projectile hits something
* test if something is currently under the mouse to simulate mouse events
* show an alert message if a spaceship is heading towards a planet

## Create the raycaster
Instantiate a raycaster with `new THREE.Raycaster()`

We can use `set(...)` method to set the `origin` and the `direction` (`Vector3`). Direction has to be normalized with `rayDirection.normalize()`
Normalize will convert the vector to a unit vector, that is, sets it equal to a vector with the same direction but with `length` of `1`

## Cast a ray
Two options:
* `intersectObject(...)` to test one object
* `intersectObjects(...)` to test an array of objects

## Result of an intersection
Always an array even if testing only one objects. This is because you can go through a single object multiple times (think of a donut). 

Each item contains useful information
* `distance` - distance between the origin of the ray and the collision point
* `face` - what face of the geometry was hit
* `faceIndex` - the index of that face
* `object` - what object is concerned by the collision 
* `point` - a `Vector3` of the exact position of the collision 
* `uv` - the UV coordinates in that geometry

## Test on each frame
If we want to test things while they're moving, we have to test each frame. 
For the lesson we'll animate spheres and turn them blue when ray intersects

We animate the spheres how we have in the past with `Math.sin(...)` in the `tick` function. 

We establish our direction and origin within the `tick` then check for intersections. We can loop over our intersect array objects and set their material color to blue. 

This, however, just turns everything blue. This is because when we start, the ray goes through all the spheres. We need to have a check that continuously updates. We can turn all the objects red before turning them blue on intersect. This give us our intended effect 

## Use the raycaster with the mouse
We can use the raycaster to test if an object is behind the moust.

We need the coordinates of the moust but not in pixels. We need a value that goes from `-1` to `+1` on horizontal and vertical axes. 

Create a `mouse` variable with a `Vector2` and update it when the mouse is moving.

use the `setFromCamera()` method to orient the ray in the right direction. The rest is the same as before.

## Mouse enter and mouse leave events
Here's the basic concept:
* create a "witness" variable containing the currently hovered object
* if an object intersects when there wasn't one before, a `mouseenter` occurs
* if no object intersects when there was one before, a `mouseleave` occurs

create the variable, then test and update the `currentIntersect` variable. We first check if anything has been pushed to the `intersects` array and, if so, then test for if our `currentIntersect` is null. If so, we have a mouse enter event. If `intersects` is empty, we then test if `currentIntersect` is exists and if so, it registers as a mouse leave event

## Mouse click event
We can now implement a `click` event and listen to it. We can then use our `currentIntersect` "witness" variable to register clicks properly. This is because the click will then only register if we are hovering over an object set to be watched.

We can also test which object is being clicked on