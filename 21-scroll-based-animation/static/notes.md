# Scroll based animations

## Goals
* We'll use Three.js as a background for plain HTML
* Make the camera follow scroll
* discover some immersive tricks
* Add a parallax animation
* trigger some animations when arriving at the corresponding sections

## HTML Scroll

### Activate the Scroll
To reactivate, remove the `overflow` line in the css

### Fix the elastic scroll
On some browsers if you scroll too far there's an elastic effect that shows the white background. We could have set the `background-color` to that of the WebGL element but instead we are going to make `clearColor` transparent and only set the `background-color` on the page itself.

Set the `alpha` prop to `true` on the `WebGLRenderer` by default, clear alpha value is `0`

## Objects
We are going to create an object for each section. Feel free to use arrays to reduce redundancy.

Remove code for cube and put three `Meshes` using Torus, Cone, and TorusKnot geometry

### Material

#### Base Material
We are going to use `MeshToonMaterial` on all three and apply the `parameters.materialColor` to the `color` prop.

### Light
We need light in order to see these materials. `MeshToonMaterial` is a type of material that only appears when light is applied.

Add one `DirectionalLight` to the scene.

### Gradient Texture
By default `MeshToonMaterial` will have on color for light and one darker color for the section in the shade. We can improve this by adding a gradient texture.

Two gradient images are provided in the static folder

Instantiate the `TextureLoader` before instantiating the `material` and load a texture provided.

Use it in the `gradientMap` prop of the material. The gradient,however, is very small and contains only 3 pixels. Instead of picking between 3 of these, we will make WebGL merge them and create the desired effect.

Set the `magFilter` of the texture to `THREE.NearestFilter`

### Position
In THREE.js the FOV is vertical. If you put one object on top, one on bottom, and then resize window the objects will stay at the top and bottom.

Create an `objectsDistance` variable and choose a random value like `2`. Increase until objects are far enough apart.

### Permanent rotation
Add the objects to a `sectionMeshes` array

in the `tick` function, loop through the `sectionMeshes` array and apply a slow rotation by using the `elapsedTime` already available.

## Camera
We need to attach the camera to the scroll and have it follow as the user scolls

First, we need to retrieve the scroll value with `window.scrollY`. Update this value by listening to `'scroll'` on the `window`

In the `tick` func, use `scrollY` to make the camera move (before the render). Negate the value so that the direction is correct. 

`scrollY` contains amount of pixels that have been scrolled. if we scroll 1000 pixels, it'll go down 1000 camera units. Each section has a `100vh`, so that means we need the camera to scroll one viewport per object, which currently has a value of `4` units between each object. To do this, we divide `scrollY` by the height of the viewport which is `sizes.height`. Then multiply this by 4 and it's perfect!

### Position object horizontally
Simply move each mesh left and right where it's needed with `mesh.position.x`

### Parallax scrolling
Parallax is the action of seeing one object through diff observation points. We are going to apply a parallax effect by making the camera move horizontally and vertically according to the mouse movements.

#### Cursor
We need to retrieve the cursor position first. Create a `cursor` object with `x` and `y` props. use the `mousemove` event on the `window` to update those values. We could use these values to move the camera but it's better to adapt them to the context. 

Instead of a value from `0` to `1`, it's better to have a value going from `0.5` to `-0.5`. to do that, subtract `0.5`

In the `tick` func create a `parallaxX` and `parallaxY` variable and put the `cursor.x` and `cursor.y` in them. We need to invert one of the cursor axes or it will move in the opposite direction of the mouse. For the scroll, the problem is that we update the `camera.position.y` twice. To fix, we put the camera in a `Group` and apply the parallax on the group and not the camera itself.

Befor creating the `camera` create the `Group` and add it to the scene, then add `camera` to the `Group`. 

In the `tick` fun apply the parallax to the `cameraGroup`

#### Easing
The parallax effect doesn't have any weight yet, the objects move fast and suddenly. To fix this, we can add some easing. On each frame, instead of moving the camera straight to target, we are going to move it a 10th closer to the destination. 

If you test the experience on a high refresh rate screen, the `tick` func will be called more often. Ideally, we want to normalize this experience across diff hardware. We need to use the time spent between each frame to fix this.

We need to find the delta time. Right after instantiating the camera, we create a `previousTime` variable. Then, in `tick` right after `elapsedTime` we calc the `deltaTime` by subtracting `previousTime` from `elapsedTime`. Then, we update the `previousTime` variable to be used on the next frame. Because `deltaTime` is in seconds, the values will be very small (around `0.016` for most common screens running at 60fps) we can change `0.1` to something like `5`. We can also lower the amplitude of the effect by multiplying our parallax values by a value less than `1`.

## Particles
particles can make a render more immersive because it adds more depth. 

We need to position the particles ourselves so we'll create a `BufferGeometry`. Create a `particlesCount` variable and a `positions` variable using a `Float32Array`
Create a loop that sets the xyz coords for each particle. Instantiate a geometry, then add a new buffer attribue and input our positions array and specify that each particle has an xyz coord with value of `3`. Then create a material with color, size, and sizeAttentuation(which allows for combined colors in perspective). Then we can create our points and input geometry and material.

For `x`(horizontal) and `z`(depth) we can use random values that can be as much positive as they are negative. For the `y`(vertical) we need to make the particles start higher than screen and end lower so that we reach the end of the scroll. We can use the `objectsDistance` variable for this. It would be good at this point to add a tweak for the color of our particles. 

You can improve our visuals as we did in the prev lessons with random sizes, random alpha and animations.

## Triggered rotations
We can now use different programming techniques to trigger rotation of the objects when they come into view.

### Knowing when to trigger the animation
We can use `scrollY` and do some math to find the current section. We can add a `currentSection` var after `scrollY` and set to `0`. we can then add a `newSection` variable to our `'scroll'` event listener and round our `scrollY / sizes.height` value to display which viewport section we are in. We can then test if the `newSection` is diff from `currentSection` and update the variables respectively.

To animate, we can use gsap and create a tween after the `currentSection` updates.
We use a `gsap.to()` and trigger our `sectionMeshes` with an array position of `currentSection`.

In our `tick` we have our rotation staticly updating based on time instead of changing the value dynamically. If we instead add to it with `deltaTime` it will be dynamic.

## Go further
We can add many things to this:
* more HTML content
* animate other properties like material
* animate the HTML texts
* Improve the particles
* add more tweaks to the Debug UI
* test other colors
* etc.