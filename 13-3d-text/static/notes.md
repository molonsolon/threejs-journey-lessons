# 3D Text
We are going to use `TextBufferGeometry` to create 3D text using typefaces

## How to get a Typeface font

you can convert your own chosen font with converters like this one: http://gero3.github.io/facetype.js/ or use the fonts from the Three.js examples folder

## How to load the font
Use the `FontLoader` with the url path sans static, other parameters are for loading logs.

## Creating the geometry
We are going to use `TextBufferGeometry`. Do not use example in docs because values are too big. We define our geometry and material within the loader, adding it to the scene as well. Test the wireframe by setting the material's `wireframe` prop to true.

Creating text geometry is long and hard for the computer. Avoid doing it many times and keep the geometry as low poly as possible by reducing the `curveSegments` and `bevelSegments` Remove the wireframe once happy with the level of detail.

