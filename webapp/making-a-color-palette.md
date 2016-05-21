# Math solution:

The following solution generates a palette for a given hue by calculating a gradient range from dark to light. Currently, we need to know three things first: The base color, the lightest color, and the darkest color. A curve is plotted along the 2d color graph using these color points.

All code displayed in this doc are pseudocode.


### Palette::generate() prototype

The pseudocode below shows a prototype for a class method that performs the said calculation. The method takes the following arguments:

- `int hue`: This is the hue for which the palette will be generated.
- `Hsb l`: This is the lightest

    class Palette
    {
        static Palette generate (Hsb b, Hsb l, Hsb d, int lb_steps, int db_steps)
    }


### Algorithm

- Establish base-color point, darkest point, and lightest point
- Plot curve along the 2d color graph 
- Perform `lb_steps` between the base-color point and the lightest point:
  - For every step, store the the color at the step as an `Hsb` object.
- Perform `db_steps` between the base-color point and the darkest point:
  - For every step, store the the color at the step as an `Hsb` object.
- Display the gathered colors
