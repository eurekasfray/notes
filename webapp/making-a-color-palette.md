# Math solution:

The following solution generates a palette for a given hue by calculating a gradient range from dark to light. Currently, we need to know three things first: The base color, the light color, and the dark color. A curve is plotted along the 2d color graph using these color points.

All code displayed in this doc are pseudocode.


### `Palette::generate()`

The pseudocode below shows a prototype for a class method that performs the said calculation.

    class Palette
    {
        public static generate(Hsv b, Hsv l, Hsv d, int lb_steps, int db_steps) {
            ...
        }
    }
    
The method takes the following arguments:

- `int hue`: This is the hue for which the palette will be generated.
- `Hsv b`: The base color. [TBD]
- `Hsv l`: The light-color point [TBD]
- `Hsv d`: The dark-color point [TBD]
- `int lb_steps`: Number of steps to take between base color and light-color point.
- `int db_steps`: Number of steps to take between base color and dark-color point.


### `Palette::generate()` algorithm

- Establish base-color point, dark-color point, and light-color point
- Plot curve along the 2d color graph 
- Perform `lb_steps` between the base-color point and the light-color point:
  - For every step, store the the color at the step as an `Hsb` object.
- Perform `db_steps` between the base-color point and the dark-color point:
  - For every step, store the the color at the step as an `Hsb` object.
- Return colors


### The `Hsv` object

This object describes a color in HSV.

    class Hsv
    {
        public Hsv(int h, int s, int v)
        {
            ...
        }
    }
    
The method takes the following arguments:

- `int h`: Hue.
- `int s`: Saturation.
- `int v`: Value.
