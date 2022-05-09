# Built-in Camera Controls

## Device Orientation Controls
Used when smart phones usually. It will automatically retrieve the device orientation if your device, OS, and browser allow it and rotate the camera accordingly.

This is useful for immersive universes or VR experiences

## Fly Controls
FlyControls enable moving the camera as if you were on a spaceship, plan, etc. Up, down, rotation.

## First Person Controls
Similar to first person controls except you can't rotate around the Y axis (perception = camera rotation)

## Pointer Lock Controls
PointerLockControls uses the pointer lock JS API. Hard to use and almost only handles the pointer lock and camera rotation. Might be better off just manually updating camera in combo w/ the aforementioned API

## Orbit Controls
Similar to the controls we made with more features like moving forward and backwards, limits vertical angle so you can't, for example, go through a rendered floor 

Make sure to read docs for full controls!!

## Trackball Controls
Like OrbitControls without the vertical angle limit

## Transform Controls
This actually has nothing to do with camera. Instead, it turns the scene into a blender-like experience with movable objects and color-coded axes. Move objects with drag controls, etc.