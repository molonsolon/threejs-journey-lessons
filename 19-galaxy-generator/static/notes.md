# Galaxy Generator
We are going to create a galaxy generator that will create a new galaxy based on GUI inputs.

## Base Particles
we are going to create a `generateGalaxy` function and place all our parameters in a object labeled `parameters`. generate a number of particles (`parameters.count`) in a  for loop. Use the same technique as last lesson where you add your count to a `Float32Array()` and multiply it by `3` within the params to cooresponsd to coordinates. Then set random positions for each by multiply your `i` value within the loop by `3` and modifying the `positions[i3]`, `positions[i3 + 1]`, and `positions[i3 + 2]`. outside the loop, set a new `position` attribute for the `geometry` that takes our `positions` array alongside `3` to represent `x`, `y`, and `z`. 

Create a new `PointsMaterial(...)` with a size parameter passed in from our `parameters` object. We mess with size attentuation, depth write, and blending but these are all just to find the right settings for your preferred look. 

Finally, create a `new THREE.Points()` with our geometry and material and add it to the scene.

## Tweaks
We'll create tweaks for the `count` and `size` params
We can then listen for changes so that a new galaxy is generated when these tweaks are modified. add a `.onFinishChange(...)` to our gui tweak and feed it our `generateGalaxy` as an arg. Our problem, however, is that our new galaxies just stack on top of the old ones when generated.

If we move the `geometry`, `material` and `points` variables outside our `generateGalaxy` function in order to test if these are null or not. Before assigning variabls within our function, we can test if they already exist and use the `dispose()` method to destroy the geometry and material property. Then remove the points from the scene with `remove()`. Make sure that you remove the `const` before the variables we created in the function previously

## Radius
We'll create a `radius` parameter for our `parameters` object. Creating the exact angles of the spirals of the galaxy, we need to get crafty with our positioning. For now, we'll create a straight line. 

## Branches
we'll add a `branches` parameter. To position the branches on our galaxy, we want to divide our particle creation into the number of branches specified. This will spread the particles out into discrete sections that we can then manipulate.

We can use the remainder operator to divide and seperate vertices. We take our `i` and  use `%` with `parameters.branches`. this will tranform our indices into repeating sets of `0` to `1 - parameters.branches`. 

In order to simplify our calculations and get small numbers, we divide this equation by the number of branches, which turns our indices into sets of values from `0` to `1` corresponding to the number of branches. To map this along a circular plane, we then can multiply this whole equation by the circumference of our circle (`Math.PI * 2`). To position our coordinates properly from our `branchAngle` value, we apply `Math.cos()` to one axis and `Math.sin()` to the other. Doesn't really matter which, but we only want 2 axes because for now we don't need any elevation variation. This creates three seperate points because it will place all of the values at our radius end points due to these functions calculation our values out to `1`. To add variation, we multiplly by our `radius` value which sets our points along our axes from `0` to the designated radius. 

## Spin
For the spin, we'll add a `spin` param. The spin will need to apply increasingly as the particles are generated farther from the center. To do this, we multiply the `radius` and the `parameters.spin` value.

We apply this to the positions of our vertices by adding our new `spinAngle` to our `x` and `z` values. Slot this angle into our cos or sin and add it to the existing `branchAngle`

## Randomness
We want to spread particles outside the exact points along our branch lines. We can create a random value for each axes and multiply it by the `radius` and `randomness` parameters. Apply these to the position! Make sure to subtract `0.5` from our random coords to allow for expansion in negative and positive directions. This still isn't convincing because it's so linear. 

A next step is to have more particles generate the closer they get to the center. In order to make this we work, we need something to modulate our particle density. We need an exponential to make this work. To do this, we can use `Math.pow(value, power)`. Apply the `Math.pow()` and multiply it by `-1` randomly to have negative values too.

## Colors
We want a color for the inner paticles and one for the outer. We can create `insideColor` and `outsideColor` params. After adding tweaks, active the `vertexColors` on the material Then we can create a section section for color in our loop using the same principles. 

Instantiate three new `THREE.Color()`, one for inside, outside, and a clone of the inside for a blend. We can't use the original because the colors would keep changing each other. We can use the `lerp(...)` method on the clone to mix it.