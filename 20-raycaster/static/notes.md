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
