# Particles
Each particle is composed of a plane (two triangles), you can have thousands of them with a reasonable frame rate. Used for stars, smoke, rain, dust, fire, etc.

Creating particles is like creating a `Mesh`. You need:
* A geometry (`BufferGeometry`)
* A material (`PointsMaterial`)
* A `Points` instance instead of a `Mesh`

## Geometry
Instantiate any `SphereBufferGeometry` Each vertex will become a particle.

## Material
Instantiate a `PointsMaterial`. Change the `size` prop to control all particle size and the `sizeAttenuation` to specify if distant particles should be smaller than closer particles

## Points
Instantiate a `Points` like we did with `Mesh`

## Custom Geometry
Instead of a `SphereGeometry`, create a `BufferGeomtry` and add a `position` attribute as we did in the Geometries lesson using `setAttribute(...)` and instantiating a `new THREE.BufferAttribute(...)` within it. 

## Color, Map, AlphaMap
Change the color of all the particles with the `color` prop on the `PointsMaterial`. Remember to instantiate a `new THREE.Color(...)` to set `color` correctly

we can use the `map` prop to put a texture on those particles. 
textures in file are resized version of the pack provided by Kenney:
https://twitter.com/KenneyNL

You can also create your own in any digital illustrator.

If you look closely, we need to activate transparency and use the texture on the `alphaMap` prop instead of the `map`. We can still see the edges of the particles, that's because the particles are drawn in the order they're created and WebGL doesn't know which one is in front of the other. There are multiple ways of fixing this.

### Using the Alpha Test
The `alphaTest` is a value between `0` and `1` that enables the WebGL to know when not to render the pixel according to the pixel's transparency. By default, the value is `0` meaning that the pixel will be rendered anyway.

## Using the Depth Test
When drawing, WebGL tests if what's being drawn is closer than what's already drawn. This is called depth testing and can be deactivated with `alphaTest`.

Deactivating the depth testing might create bugs if you have other objects in your scene or particles with different colors.

## Using Depth Write
The depth of what's being drawn is stored in a depth buffer. Instead of not testing if the particle is closer than what's in this depth buffer, we can tell the WebGL not to write particles in that depth buffer with `depthWrite`. 


There's no perfect solution, you'll have to adapt and find the best combination according to the projet.

## Blending
WebGL currently draws pixels one on top of the other. With the `blending` prop we can tell WebGL to add the color of the pixel to the color of the pixel already drawn. 
Change the `blending` prop to `THREE.AdditiveBlending`. This effect will impact performance.

## Different Colors
We can have a different color for each particle. Add a `color` attribute with 3 values (red, green, and blue). Then, you can use the same loop we used for positions to filla new `Float32Array(...)` and apply a new color attribute to the geometry

## Animation
There are multiple ways to animate particles

### Using Points as an Object
`Points` class inherits from `Object3D` so we can move, rotate, and scale the points.

 ### Changing the Attributes
 we can update each vertex seperately in the `particlesGeometry.attributes.position.array` Because this array contains the particles positions, we have to go `3` by `3`

 For this example, we'll make the particles move up and down like waves. To create waves, we can use a `Math.sin()`. Nothing moves because Three.js needs to be notified when a geometry attribute changes. Set the `needsUpdate` to `true` on the `position` attribute. Apply an offset to the sin so that you get that wave shape. we can use the `x` coordinate. This works! However...
 You should avoid this technique because updating the whole attribute on each frame is terrible for performance

 ### Using a Custom Shader
 This is the best way to animate particles, but we won't learn this until later.