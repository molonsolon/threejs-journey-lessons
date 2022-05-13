# Lights
adding lights is as simple as adding meshes. We instantiate a light class and add it to the scene.

## Ambient Light
Applies omnidirectional lighting. Props are `color` and `intensity`. Omnidirectional means that the light is coming in from all directions, thus creating the effect of even no matter what the shape of the mesh. 

We can use `AmbientLight` to simulate light bouncing. Adding a small amount will allow for more realistic lighting because directional lights do not account for natural light bouncing.

## Directional Light
This will have a sun-like effect as if the sun rays were traveling in parallel. Also has the `color` and `intensity` props.

To change the direction, move the light with `position.set(...)`. Distance does not matter for now.

## Hemisphere Light
Similar to `AmbientLight` but with a different color from the sky than the color coming from the ground. the props are:
* `color` (or `skyColor`)
* `groundColor`
* `intensity`

`skyColor` determines the color of light coming from directly above (positive y) and `groundColor` determines the color of light coming from directly below (negative y).
works like ambient light because, while direction, it's coming from everywhere.

## Point Light
`PointLight` is almost like a lighter. Starts at an infinitely small point and spread uniformly in every direction. props: `color` and `intensity`.

can be moved with `position` prop. 

By default, the light intensity doesn't fade as it gets further away. We can change this with the `distance` and `decay` props. 
`distance` is maximum range of light (default 0, no limit)
`decay` is the amount of light that dims along the distance of the light. Default is 1. For physically correct lighting, set this to 2.

## Rect Area Light
`RectAreaLight` works like lightboxes in a photoshoot. It's a mix between directional and diffuse light. The props are:
* `color`
* `intensity`
* `width`
* `height`

Only works with `MeshStandardMaterial` and `MeshPhysicalMaterial`

Move light and rotate. You can also use `lookAt(...)` to rotate more easily

## Spot Light
`SpotLight` is like a flashlight. It's a cone of light starting at a point and oriented in a direction. props are:
* `color`
* `intensity`
* `distance`
* `angle`
* `penumbra`
* `decay`

To rotate the spotlight, we need to add its `target` property seperately to the scene and move it. 

## Performance
Lights come at a high performance cost. Try to add as few lights as possible and try to use lights that are less costly. 

### Minimal Cost
* `AmbientLight`
* `HemisphereLight`

### Moderate Cost
* `DirectionalLight`
* `PointLight`

### High Cost
* `SpotLight`
* `RectAreaLight`

## Baking
To allow for more light processing, you can bake the light into the texture. This can be done in a 3D software. The drawback is that we cannot move the light anymore and we have to load huge textures.

Examples:
* https://threejs-journey.com/assets/lessons/15/013.jpg

## Helpers
To assist with positioning lights, we can use helpers.
Each of the covered classes have an associated helper class.
`HemisphereLightHelper`, `DirectionalLightHelper` and `PointLightHelper` are instantiated with props of `light`, `size` and `color`, the latter being optional as it will by default take the color of the passed-in light.

`SpotLightHelper` has no `size` and we need to call its `update(...)` method on the next frame after moving the target.

`RectAreaLight` isn't part of the `THREE` variable and must be manually imported.


