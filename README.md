# week4_cpp

## Overview

### Hue (h)

``` h = [0,360] ```

Two main color:
- "Illini Orange" has a hue of 11
- "Illini Blue" has a hue of 216

### Saturation (s)

``` s = [0,100] ```

Non-color --> Full Color

### Luminance (l)

``` l = [0,100] ```

White --> Dark

### HSL Color Space

<p align="justify">
<div class="text">
The full HSL color space is a three-dimensional space, but it is not a cube (nor exactly cylindrical). The area truncates towards the two ends of the luminance axis and is widest in the middle range. The ellipsoid reveals several properties of the HSL color space:
</div></p>

- At l=0 or l=1 (the top and bottom points of the ellipsoid), the 3D space is a single point (the color black and the color white). Hue and saturation values don’t change the color.
- At s=0 (the vertical core of the ellipsoid), the 3D space is a line (the grayscale colors, defined only by the luminance). The values of the hue do not change the color.
- At s=1 (the outer shell of the ellipsoid), colors are vivid and dramatic!

## Part 1: Programming Objectives

You need to create a class called **HSLAPixel** within the uiuc namespace in the file **uiuc/HSLAPixel.h**. Each pixel should contain four public member variables:
- h, storing the hue of the pixel in degrees between 0 and 360 using a double
- s, storing the saturation of the pixel as a decimal value between 0.0 and 1.0 using a double
- l, storing the luminance of the pixel as a decimal value between 0.0 and 1.0 using a double
- a, storing the alpha channel (blending opacity) as a decimal value between 0.0 and 1.0 using a double

That's it! Once you have this simple class, you are ready to compile and test Part 1 of this assignment.