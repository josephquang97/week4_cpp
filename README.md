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

## Part 2: Using uiuc::PNG

</p class="text">
<div align="justify">Now that we have an HSLAPixel, it's time to manipulate some images! First, let's understand the PNG class that
is provided for us! Note that PNG stands for "portable network graphics." It's a useful image file format for
storing images without degradation (called losssless compression). A photograph saved as PNG will have a
much larger filesize than a JPEG, but it will also be sharper and the colors won't bleed together as happens with
JPEG compression. Apart from photographs, though, PNG offers relatively small filesizes for simple graphics
with few colors, making it a popular choice for small details in a website user interface, for example.</div>
</p>
<p class="text">
<div align="justify">Inside of your uiuc directory, you may have noticed <b>PNG.h</b> and <b>PNG.cpp</b>. We have provided an already
complete PNG class that saves and loads PNG files and exposes a simple API for you to modify PNG files. (An
API, or application programming interface, just means the documented, surface-level part of the code that you
work with as someone who is making use of the library. You do not need to fully understand how the PNG class
works underneath in order to use it. But, it's good for you to practice surveying library code, seeing how it is
organized, and taking note of its intended usage.)</div>
</p>

### For loading and saving PNG files:

- **bool PNG::readFromFile(const std::string & fileName)** loads an image based on the provided
file name, a text string. The return value shows success or failure. The meaning & is discussed in Week 3
in the lecture about variable storage; it means a direct reference to the memory is being passed, similar
to a pointer.
- **bool PNG::writeToFile(const std::string & fileName)** writes the current image to the provided
file name (overwriting existing files).

### For retrieving pixel and image information:

- **unsigned int PNG::width() const** : returns the width of the image.
- **unsigned int PNG::height() const** : returns the height of the image.
- **HSLAPixel & getPixel(unsigned int x, unsigned int y)** : returns a direct reference to a pixel at
the specified location

### Other methods:

- Methods for creating empty PNGs, resizing an image, etc. You can view **uiuc/PNG.h** and
**uiuc/PNG.cpp** for complete details.

### Sample Usage of PNG

<p class="text"><div align="justify">
Suppose we want to transform an image to grayscale. Earlier you learned that a pixel with a saturation set to 0%
will be a gray pixel. For example, here’s this transformation applied to the Alma Mater. The image on the left is
the original, and the one on the right has been desaturated:
</div></p>

<p align="justify">
<img src="./.github/example1.svg?sanitize=true">
</p>

<p class="text"><div align="justify">
The source code that used the PNG class to create this grayscale transformation above is as follows (which is
also provided to you in ImageTransform.cpp):
</div></p>

```cpp
/**
* Returns an image that has been transformed to grayscale.
* The saturation of every pixel is set to 0, removing any color.
* @return The grayscale image.
*/
PNG grayscale(PNG image) {
/// This function is already written for you so you can see how to
/// interact with our PNG class.
for (unsigned x = 0; x < image.width(); x++) {
for (unsigned y = 0; y < image.height(); y++) {
HSLAPixel & pixel = image.getPixel(x, y);
// `pixel` is a reference to the memory stored inside of the PNG `image`,
// which means you're changing the image directly. No need to `set`
// the pixel since you're directly changing the memory of the image.
pixel.s = 0;
}
}
return image;
}
```

<p class="text"><div align="justify">
Here the pixel itself is passed by reference, although for simplicity, the image manipulation functions
themselves in <b>ImageTransform.cpp</b> simply pass the images by value, which means they pass an entire new
copy of the image each time. In the future, in practice, you'll find that it's more efficient to indirectly refer to large memory objects using pointers and references instead of making extra copies just to pass between
functions. Many scripting languages today use reference semantics for almost everything by default, but C++
gives you more control to choose when memory is copied or when it is altered in-place.
</div></p>

## Part 3 Programming Objectives

<p class="text"><div align="justify">
For the rest of the assignment, you should finish implementing the functions in <b>ImageTransform.cpp</b>: <b>illinify</b>, <b>spotlight</b>, and <b>watermark</b>. You'll find that the grayscale function discussed above has already been
completed for you. There are some comments on the function bodies with notes about their implementation.
You may want to make use of some basic mathematical functions that are available because of the
<b>#include `<`cmath`>`</b> in <b>ImageTransform.h</b>, but that's not required.
</div></p>

<p class="text"><div align="justify">
As you know, all C++ programs begin their execution with the main function, which is usually defined in
<b>main.cpp</b>. You can find that a main function has been provided for you that:</div></p>
- Loads in the image alma.png
- Calls each image modification function
- Saves the modified images named as out-modification.png, where modification shows the function being tested (e.g. out-grayscale.png)

Descriptions and examples of the three remaining functions are given below:

#### Function #1: illinify

<p class="text"><div align="justify">
To illinify an image is to transform the hue of every pixel to Illini Orange (11) or Illini Blue (216). The hue of
every pixel should be set to one or the other of these two hue values, based on whether the pixel's original hue
value is closer to Illini Orange or Illini Blue. Remember, hue values are arranged in a logical circle! If you keep
increasing the hue value, for example, what should eventually happen?
</div></p>

<p align="justify"><img src="./.github/example2.svg?sanitize=true"></p>

#### Function #2: spotlight

<p class="text"><div align="justify">
To spotlight an image is to create a spotlight pattern centered at a given point (<b>centerX, centerY</b>).</div></p>
<p class="text"><div align="justify">
A spotlight adjusts the luminance of a pixel based on the distance between the the pixel and the designated
center by decreasing the luminance by 0.5% per 1 pixel unit of Euclidean distance, up to an 80% decrease in
luminance at most.</div></p>
<p class="text"><div align="justify">
For example, a pixel 3 pixels above and 4 pixels to the right of the center is a total of <b>sqrt</b>(3*3 + 4*4)
= <b>sqrt</b>(25) = 5 pixels away and its luminance is decreased by 2.5% (0.975 its original value). At a distance
over 160 pixels away, the luminance will always be decreased by 80% (0.2x its original value).
</div></p>

<p align="justify"><img src="./.github/example3.svg?sanitize=true"></p>

#### Function #3: watermark

<p class="text"><div align="justify">
To watermark an image is to lighten a region
of it based on the contents of another image
that acts as a stencil.</div></p>

<p class="text"><div align="justify">
You should not assume anything about the
size of the images. However, you need only
consider the range of pixel coordinates that
exist in both images; for simplicity, assume
that the images are positioned with their
upper-left corners overlapping at the same
coordinates.</div></p>

<p class="text"><div align="justify">
For every pixel that exists within the bounds
of both base image and stencil, the luminance of the base image should be increased by +0.2 (absolute, but not
to exceed 1.0) if and only if the luminance of the stencil at the same pixel position is at maximum (1.0)
</div></p>

<p align="justify"><img src="./.github/example4.svg?sanitize=true"></p>

