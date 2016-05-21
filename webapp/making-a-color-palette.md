# Solution by hand:

## Steps

0. Establish base point, light point, dark point in HSV (format: `HSV(h,s,v)`; example: `HSV(225,2,97)`)
0. Find difference between corresponding HSV values of the base point and light point.
0. Find diff by dividing the result by `n` to get the number of steps to take between the base point and light point.
0. Plot range by applying the diff to each value in HSV for `n` steps.
0. Repeat steps A - B for dark point (where in this case you substract for each step)


## Example

    1. Establish points
    
       base point  = HSB(255,057,060)
       light point = HSB(225,002,097)
       dark point  = HSB(225,025,020)
       
    2. Find difference between base point and other points
    
       Find difference between base point and light point:
       base  :   225 057 060
       light : - 225 002 097
       ---------------------
       diff  : = 000 055 037
       ---------------------
       
       Find difference between base point and dark point:
       base  :   225 057 060
       dark  : - 225 025 020
       ---------------------
       diff  : = 000 032 040
       ---------------------
       
    3. Divide diff by steps. I choose 4 steps.
       Divide H, S & V by the given steps
    
       Light-point diff:
         000 055 037 div 4
       = 000 014 009
    
       Dark-point diff:
         000 032 040 div 4
       = 000 008 010
       
    4. Plot values
    
       Plot for range from base point to light point by adding the diffs for `n` steps.
       Add hue*, substract saturation, add value:
       
       Init: 
            ------  ---  ---  ---
            HSV     255  057  060
            ------  ---  ---  ---
            
       Step 1: 
            HSV     255  057  060
            diff   +000 -014 +009
            ------  ---  ---  ---
            result  255  043  069
            ------  ---  ---  ---
            
       Step 2: 
            HSV     255  043  069
            diff   +000 -014 +009
            ------  ---  ---  ---
            result  255  029  078
            ------  ---  ---  ---
            
       Step 3: 
            HSV     255  029  078
            diff   +000 -014 +009
            ------  ---  ---  ---
            result  255  015  087
            ------  ---  ---  ---
            
       Step 4: 
            HSV     255  015  087
            diff   +000 -014 +009
            ------  ---  ---  ---
            result  255  001  096
            ------  ---  ---  ---
    
       Plot for range from base point to dark point by subtract the diffs for `n` steps:
       Substract hue*, add saturation, subtract value:
       
       Init: 
            ------  ---  ---  ---
            HSV     255  057  060
            ------  ---  ---  ---
            
       Step 1: 
            HSV     255  057  060
            diff   -000 +008 -010
            ------  ---  ---  ---
            result  255  065  050
            ------  ---  ---  ---
            
       Step 2: 
            HSV     255  065  050
            diff   -000 +008 -010
            ------  ---  ---  ---
            result  255  073  040
            ------  ---  ---  ---
            
       Step 3: 
            HSV     255  073  040
            diff   -000 +008 -010
            ------  ---  ---  ---
            result  255  081  030
            ------  ---  ---  ---
            
       Step 4: 
            HSV     255  081  030
            diff   -000 +008 -010
            ------  ---  ---  ---
            result  255  089  020
            ------  ---  ---  ---
       
       * I don't know whether to add/substract hue. What do I do with the hue?
    

# Math solution:

The following solution generates a palette for a given hue by calculating a gradient range from dark to light. Currently, we need to know three things first: The base color, the light color, and the dark color. A curve is plotted along the 2d color graph using these color points.

All code displayed in this doc are pseudocode.


## `Palette::generate()`

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


### Algorithm

- Get base-color point, dark-color point, and light-color point
- Plot curve along the 2d color graph using the points above
- Create new `Swatch` object and store it as `swatch`
- Perform `lb_steps` between the base-color point and the light-color point:
  - For every step, store the the color at the step as an `Hsb` object in `swatch`
- Perform `db_steps` between the base-color point and the dark-color point:
  - For every step, store the the color at the step as an `Hsb` object in `swatch`
- Return `swatch`


## The `Hsv` object

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
