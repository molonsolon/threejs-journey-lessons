# Materials

Materials are used to put a color on each visible pixel of a geometry. The algorithms are written in programs called shaderes. We don't need to write shaders for now and can instead use built-in materials

## Color
the `color` property will provide a uniform color on the surface of the geometry and must be instantiated with a Color class: `new THREE.Color('#ff0000')`. This color will combine with the given texture and tint it appropriately.

## Wireframe
Transform a material into a wireframe with `material.wireframe = true`

## Opacity
In order to set opacity, you must first set `material.transparent` to true. then you can do something like `material.opacity = 0.5`

## AlphaMap
`alphaMap` controls the transparency with a given texture. can be combined with textures to make certain parts transparent

## Side
`side` lets you decide which side of a face is visible.
* `THREE.FrontSide` (default)\
* `Three.BackSide`
* `THREE.DoubleSide` (try to avoid using because it's GPU heavy)


## Mesh Normal Material 
Normals are information that contains the direction of the outside of the face. Normals can be used for lighting, reflection, refraction, etc.

Shares common properties with `MeshBasicMaterial` like `wireframe` `transparent` `opacity` and `side` but there is also a `flatShading` property. This will flatten faces meaning that the normals won't be interpolated between the vertices.

This is usually used to debug normals, but the color looks so great that you can use it for your projects.

## Mesh Matcap Material
will display a color by using the normals as a reference to pick the right color on a texture that looks like a sphere. Set the matcap texture with the `matcap` property. Simulates the behvaior of light without a lightsource.

* great matcap repo: https://github.com/nidorx/matcaps

create your own with 3D software by applying color, a light source, then taking a statics square image render to use as the matcap. Can do it manually in photoshop but it's not as consistent.

## Mesh Depth Material
Will simplyl color the geometry in white if it's close to the `near` and in black if it's close to the `far` value of the camera.

## Adding a few lights
We create an `AmbientLight` and a `PointLight` to see the next materials. The latter can be given a specific location.

## Mesh Lambert Material
This material specifically reacts to lights. It has new props but we will see those later with a more adequate material. It's performant but we can see strange patterns on the geometry.

## Mesh Phong Material
similar to `MeshLambertMaterial` but the strange patterns are less visible and you can see the light reflection. Less performant than `MeshLambertMaterial`

we can control the light reflection with `shininess` and the color of this reflection with `specular` props

## Mesh Toon Material
similar to `MeshLambertMaterial` but more cartoonish. 

To add more steps to the coloration, you can use `gradientMap` and `gradientTexture`

We see a gradient instead of a clear seperation because the gradient is small and the `magFilter` tries to fix it with the `mipmapping` 

Set the `minFilter` and `magFilter` to `THREE.NearestFilter` We can also deactivate the mipmapping with `gradientTexture.generateMipmaps = false`

## Mesh Standard Material
This is the one of the most usable materials. It uses phsyically based rendering principles (PBR). Like Lambert and Phong, it supports light but with more realistic algorightms and better parameters like `roughness` and `metalness`.

`map` prop allows you to apply a texture here as well.

`aoMap` ('ambient occlusion map') will add shadows where the texture is dark. we must add a second set of UV coordinates named `uv2`. 
In our case, it's the same coordinates as the default UV so we are going to reuse it. Make sure to establish that each vertex is defined by 2 points, declared using the 3rd parameter of the `THREE.BufferAttribute(...)` using `2`. 

Control the intensity with `aoMapIntensity`

`displacementMap` will move the vertices to create relief. It looks terrible because it lacks vertices and the displacement is way too strong.

instead of specifying uniform `metalness` and `roughness` for the whole geometry, we can use `metalnessMap` and `roughnessMap`.

Reflection still looks weird because props still affect each map respectively. Comment out `metalness` and `roughness` or use their original values.

`normalMap` will fake the normals orientation and add details on the surface regardless of subdivision.

change the normal intensity with the `normalScale` property (Vector2).

Finally, we can control the alpha using the `alphaMap` property. Don't forget `transparent = true`

## Mesh Physical Material 
Same as `MeshStandardMaterial` but with support for a clear coat effect.

## Points Material
You can use `PointsMaterial` with particles

## Shader Material and Raw Shader Materials
Use when you want to create your own materials

## Environment Map
an image of what's surrounding the scene. It can be used for reflection or refraction but also for general lighting. Supported by multiple materials but we are going to use `MeshStandardMaterial`

Three.js only supports cube environment maps. You can find multiple environment maps in the `/static/` folder.

To load a cube texture, we must use the `CubeTextureLoader` instead of `TextureLoader`

Instantiate the `CubeTextureLoader` before instantiating the `material` and call its `load(...)` method but use an array of paths (look at code for specific order needed)

use the `environmentMapTexture` with the `envMap` property to apply

You can tweak the `metalness` and `roughness` for different results and test other environment maps for the `/static/` folder.

### Where to find environment maps?
you can always do simple web searches but make sure you have the right to use them.

There's also HDRIHaven:
https://hdrihaven.com/
Hundreds of awesome HDRIs with CC0 license which means you have free use to use them for any kind of work. You can thank them by subbing to their patreon. 

To convert these HDRIs to cube maps, use this website: 
https://matheowis.github.io/HDRI-to-CubeMap/