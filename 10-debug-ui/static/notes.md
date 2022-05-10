# Tweaks
* `Range` - fro numbers with minimum and maximum value
* `Color` - for colors with various formats
* `Text` - for simple texts
* `Checkbox` - for booleans
* `Select` - for choice from a list of values
* `Button` - to trigger functions
* `Folder` - to organize your panel if you have too many elements

# Add elements

Use `gui.add(...)` to add an element (a `tweak`)

* first param is an object
* second is property you want to tweak

next parameters handle:
* minimum
* maximum
* step (precision)

must add after variable to tweak has been declared
we can also use `min(...)`, `max(...)`, and `step(...)` to edit these params

dat.gui will change type of tweak according to type of property listed. i.e. will add a checkbox if boolean property is given

## Color

for color, you must use `.addColor(...)` to create the proper gui element
HOWEVER we need a workaround for accessig this parameter in the material object because of the way the object is formatted.

We use `onChange(...)` method to know when the tweak value changed and `material.color.set(...)` to update the color because material.color is an instance of Color

declare a paramaters object with a key of color and use parameters.color directly on the material declaration

## Functions

to trigger a function, store it in an object. we can use the parameters object again and create a spin method

# Tips

## Hide
press H to hide the debug panel. if you want the panel to be hidden at start, use gui.hide()

## Close
click close to collapse options. if you want it to start closed, send an object when instantiating and pass `closed: true`

## Width
you can drag and drop panel to change width
you can change the default with `width` param on the gui instatiantion.

## VITAL
Always add gui elements as you're coding!! otherwise it will take too much extra time to go back through and find all the params you want to manipulate

# Extra Info
check this out for a full example, includes import/export-able presets
https://jsfiddle.net/ikatyang/182ztwao/