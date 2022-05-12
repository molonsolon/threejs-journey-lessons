# 3D Text
We are going to use `TextBufferGeometry` to create 3D text using typefaces

## How to get a Typeface font

you can convert your own chosen font with converters like this one: http://gero3.github.io/facetype.js/ or use the fonts from the Three.js examples folder

## How to load the font
Use the `FontLoader` with the url path sans static, other parameters are for loading logs.

## Creating the geometry
We are going to use `TextBufferGeometry`. Do not use example in docs because values are too big. We define our geometry and material within the loader, adding it to the scene as well. Test the wireframe by setting the material's `wireframe` prop to true.

Creating text geometry is long and hard for the computer. Avoid doing it many times and keep the geometry as low poly as possible by reducing the `curveSegments` and `bevelSegments` Remove the wireframe once happy with the level of detail.

## Center the text
There are multiple options for centering the text

### Using the bounding
The bounding is info associated with the geometry that tells what space is taken by that geometry. It can be a box or a sphere.

Bounding helps three.js calculate if the object is on the screen (frustum culling).
It can be used to recenter the geometry. Frustum culling is useful to avoid computation of things that are not visible. By default, Three.js uses sphere bounding. calc the box bounding with `computeBoundingBox()`. The result is an instance of Box3 with `min` and `max` props. the `min` isn't at `0` because of the `bevelThickness` and `bevelSize`. 

Instead of moving the mesh, we are going to move the whole geometry with `translate(...)` You multiply each `max` value of the bounding box by `0.5` in order to move it over by exactly half the bounding box. This text looks centered but it's not because of the `bevelThickness` and `bevelSize`. To solve, subtract the value of `bevelSize` from the x and y and `bevelThickness` from the z.

Well... the above method is the long, manual way. To do this simply, just use the `center()` method on the geometry.

## Add a matcap material
Use the given matcaps in `/static/textures/matcaps/` or you can download one from:
https://github.com/nidorx/matcaps 

Load the texture with `TextureLoader`, use `MeshMatcapMaterial` and apply the loaded `matcapTexture` variable with the `matcap` prop. 

## Add Objects
Create 100 donuts within the success function of the texture loader. We can add randomness in the position of these donuts before adding to the scene by modifying the `position` coordinates and `Math.random()`. Do the same for two axes on the rotation. Then, create a variable with  single random value and apply that to all axes for the `scale` prop.

## Optimize
Our code nees optimization because of the slow loading time. We can use the same material and same geometry on multiple meshes. Move the geometry and material of the donuts outside of the loop function.

Go further by using the same material as the `text`. 