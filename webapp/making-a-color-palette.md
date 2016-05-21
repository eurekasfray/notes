# Math solution:

The following solution generates a palette for a given hue by calculating a gradient range from dark to light. The hue remains constant while the saturation and value changes. Currently, we need to know three things first: The base color, the lightest color, and the darkest color.

All code displayed in this doc are pseudocode.


### Algorithm

- Get hue. The hue color

- Establish darkest point, midtone point, and lightest point
- Plot curve along the 2d color graph 
- Determine steps to take between darkest point and midtone point
- Determine steps to take between midtone point and lightest point
- Perform step across the established ranges
- Display the gathered colors


### Palette::generate() prototype

The pseudocode below shows a prototype for a class method that performs the said calculation. The method takes the following arguments:

- `int hue`: This is the hue for which the palette will be generated.
- `Hsb l`: This is the lightest

    class Palette
    {
        static Palette generate (int hue, Hsb l, Hsb m, Hsb d, int lm_steps, int dm_steps)
    }
