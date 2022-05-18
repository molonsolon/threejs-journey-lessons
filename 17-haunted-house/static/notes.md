# Haunted House
In this lesson, we're going to use only Three.js primitives and the textures in the `/static/textures/` folder

## Tips for measurements
Instead of using random measures, we are going to consider `1 unit` as `1 meter`

## Fog
Three.js supports fog with the `Fog` class
* `color`
* `near` - how far from the camera the fog starts
* `far` - how far from the camera will the fog be fully opaque

to fix the background, we must change the clear color of the `renderer` and use the same color as the fog. We can do this with the `setClearColor(...)` on the renderer.

## Texture Repeats
To control the amount of repeats, access the `repeat` prop directly on the texture then modulate it's `x`, `y`, or `z` props individually or with `.set(...)`

To control the repeating nature of textures, access the textures directly on their `wrapS` and `wrapT` props and set them to `THREE.RepeatWrapping`