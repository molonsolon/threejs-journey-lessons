# What is a geometry?
A collection of vertices that are connected by lines. Each geometry is a discrete set of vertices. Particles can be rendered by not connecting these vertices with lines. Each vertex can be manipulated in many ways in this case.

# Built-in geometries
All the following geomoetries inherit from `BufferGeometry`.
This class has many built in methods that should be examined in the docs. 
Combine these geometries to create complex shapes.

## BoxGeometry
This is simply a box that can be manipulated into different rectangular prisms. 

## PlaneGeometry
Two triangles that create a flat plane

## CircleGemotry
A series of triangles forms a circle. Has many paramters to manipulate.

## ConeGeometry
A series of triangles that form a cone.

## CylinderGeometry
A series of triangles that form a cylinder

## RingGeometry
A series of triangles that form a flat ring

## TorusGeometry
Forms a 3D ring

## TorusKnotGeometry
Forms a infinite knot that is good for testing lighting and environment parameters

## DodechahedronGeometry
A 12 faced 3D shape

## OctahedronGeometry
A 8 faced 3D object

## TetrahedronGeometry
Creates a 4 faced 3D object

## IcosahedronGeometry
create a sphere composed of triangles that have roughly the same size

## SphereGeometry 
Creates a typical sphere

## ShapeGeometry 
Uses Bezier curves to create custom shapes

## TubeGeometry
Creates a tube that follows a specified path

## ExtrudeGeometry
Creates a beveled shape

## LatheGeometry
Provide values for a single line that then draws a revolution, i.e. a lampshade

## TextGeometry
Creates 3D text based on typed input

# Box Example
Has 6 parameters:
`height`, `width`, `depth`, `widthSegment`, `heightSegment`, `depthSegment`.
the latter 3 determine the number of triangles on each face.
A value of 1 on these segments creates 2 triangles, 2 = 8 triangles.

If you want to create terrain, for example, you would want to create many triangles to add depth and detail. The process of adding triangles is called `subdivision`.

## Wireframes
You can add `wireframe: true` to your mesh but beward that it will have a set line width and may show more staircasing 

## Creating a BufferGeometry
in order to store buffer geometry data, we need to use a `Float32Array`
* typed array
* can only store floats
* fixed length (can only store as many values as you specify)
* easier to handle for computer when this is used

Two ways of creating and filling a `Float32Array`
Each vertex is established by 3 points that correspond to x,y,z coordinates respectively. For a position vertex, it's 3 coordinates, for UV it's 2, for establishing particle size, it would be 1. 

With an established array, we can convert the array to a `BufferAttribute`.

Why use `position` attribute when `setAttribute(...)`? Three.js internal shaders will look for that value to position the vertices. More about that in shaders lesson.

As you can see from result, you don't have to establish a face. Three.js compiles the BufferGeometry automatically to account for this.

for random triangle creation, you multiply the count by 3 twice because each triangle needs 3 sets of x,y,z coordinates 

Some geometries have faces that share common vertices. When creating a `BufferGeometry` we can specify a bunch of vertices and then the indices to create the faces and re-use the vertices multiple times. Covered in depth later!
