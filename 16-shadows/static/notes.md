# Shadows
Core shadows (the areas that our current lights don't hit) are established from the previous lessons, but we need to add drop shadows. 

##  How Three.js shadows work
When you render, three.js will do a render for each light supporting shadows. those renders will simulate when the light sees as if it was a camera. 
During these light renders, a `MeshDepthMaterial` replaces all meshes material.
The light renders are stored as textures called shadow maps
they are then used on every material that recieves shadows and projects them on the geometry.

## How to activate shadows
Activate the shadow maps on the `renderer` by setting the `shadowMap.enabled` prop to true. 

Then, add a `true` value to `castShadow` and `recieveShadow` props on whatever objects are casting or receiving shadows respectively.

Only the following types of light support shadow:
* `PointLight`
* `DirectionalLight`
* `SpotLight`

set the `castShadow` prop on the support lights to `true` to fully activate shadows.

## Optimize shadows

### Render Size
Shadow maps have width and height that can be accessed to improve shadow render quality. 
Access these resolutions with `directionalLight.shadow.mapSize.width` or `height` respectively. By default, it's set to `512x512`. Improve but always keep i at a power of 2 for the purpose of mipmapping

### Near and Far
To help debug, use a `CameraHelper` with the camera used for the shadow map located in `directionalLight.shadow.camera` This shows us that by default, the `far` param is way too far. Set the `near` and `far` on the `shadow.camera`.

## Amplitude
Because we are modifying a `DirectionalLight`, Three.js uses an `OrthographicCamera`. We can control how far on each side the camera can see with `top`, `right`, `bottom` and `left` props.  The smller the values, the more precise the shadows will be. If too small, the shadow will be cropped

Once lighting has been optimized, hide the camera helper by setting the `visible` prop to false.

## Blur
Control the shadow blur with the `radius` prop. Doesn't use the proximity of the camera with the object, it's general and cheap blur.

## Shadow map algorithms
There are four shadow map algorithms to choose from:
* `THREE.BasicShadowMap` - performant but bad quality
* `THREE.PCFShadowMap` - less performant but smoother edges (default)
* `THREE.PCFSoftShadowMap` - less performant but even softer edges
* `THREE.VSMShadowMap` - less performant, more constraints, can have unexpected results

These can be set with `renderer.shadowMap.type`. `THREE.PCFSoftShadowMap` provides a decent detail in the shadow but you cannot use a blur `radius`

## Spotlight
as always, set the `castShadow = true` and add both the `spotLight` and `spotLight.target` to the scene. remember to add a camera helper with `spotLight.shadow.camera` as the prop. It doesn't look great in our current render but turning down the intensity of other lights can help.

Improve shadow quality using the same techniques as we did with `directionalLight`
Change `shadow.mapSize`.

`SpotLight` uses a `PerspectiveCamera` we must change the `fov` to adapt the amplitude, then the `near` and `far`.

## Point Light
Set up this light and the camera helper the same as we did in the previous two.
Camera helper is a `PerspectiveCamera` facing downward. It is bound by 6 points and faces downward because it needs to have a good reference to it's surrounding for shadows, points in all directions. 

To optimize, we can tweak `mapSize`, `near` and `far`.

## Baking Shadows
A good alternative to Three.js shadows is baked shadows. This involves integrating the shadows into the texture themselves. 

To swap these, we can deactivate all shadows from the renderer with `renderer.shadowMap.enable = false`

instead of `MeshStandardMaterial` use `MeshBasicMaterial` on the plane material with the `bakedShadow` texture. Unfortunately, this is static (unresponsive to movement)

## Baking Shadows Alternative
we can also use a simpler baked shadow and move it so it stays under the sphere. That way, when the sphere moves, the plane follows. When the sphere gets farther away, we reduce alpha, closer to plane increase alpha. 
You can create a plane slightly above the floor with an `alphaMap` using the `simpleShadow`, making sure to set the `transparent` value to `true`

Animating the sphere in this lesson involves using the `Math.abs()` function which keeps whatever values applied positive.
