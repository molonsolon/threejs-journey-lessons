# Common Textures

## Color (or Albedo)
* most simple
* applied to geometry 
![](https://threejs-journey.com/assets/lessons/11/000.jpg)

## Alpha
* grayscale image
* white visible
* black no visible
![](https://threejs-journey.com/assets/lessons/11/001.jpg)

## Height (or Displacement)
* greyscale 
* move the vertices to create some relief
* need enough subdivision
![](https://threejs-journey.com/assets/lessons/11/002.png)

## Normal
* adds detaiil
* doesn't need subdivision
* vertices won't move
* lure the light about the face orientation
* better performance than adding a height texture with a lot of subdivision
![](https://threejs-journey.com/assets/lessons/11/003.jpg)

## Ambient Occlusion
* grayscale image
* add fake shadows in crevices
* not physically accurate
* helps to create contrast and see details
![](https://threejs-journey.com/assets/lessons/11/004.jpg)

## Metalness
* grayscale image
* white is metallic
* black is non-metallic
* mostly for reflection
![](https://threejs-journey.com/assets/lessons/11/005.jpg)

## Roughness
* grayscale image
* often in combo with metalness
* white is rough
* black is smooth
* mostly for light dissipation
![](https://threejs-journey.com/assets/lessons/11/006.jpg)

## PBR
These textures (esp metalness and roughness) follow PBR principles

* Physically based rendering
* many techniques that tend to follow real-life directions to get realistic results
* becoming standard for realistic renders
* many software, engines, and libraries are using it

for more info: 
*  https://marmoset.co/posts/basic-theory-of-physically-based-rendering/
* https://marmoset.co/posts/
physically-based-rendering-and-you-can-too/

# How to load textures

## Getting URL of the image
To load texture, we need URL of the image file. With webpack, there are two ways of doing this

1. Put the image texture in the `/src/` folder and import

2. Put the image in the `/static/` folder and access it directly. This is the technique we will use

## Loading the image
There are multiple ways to load an image

### Using native JavaScript
Create an `Image` instance, listen to the `load` event, and change its `src`.

Create a `texture` variable with the Texture class
We need to use that `texture` in the `material`.
Unfortunately, the `texture` variable cannot be accessed directly because it's inside our `onload(...)` function.