---
title: "Display"
nodateline: true
weight: 1
---

The *display* module is available on platforms which have the *framebuffer* driver enabled. It allows for controlling the display of your device.
{{% notice tip %}}
**Available on:** &nbsp;&nbsp; ✅ [CampZone 2020](/badges/campzone-2020/) &nbsp;&nbsp; ✅ [Disobey 2020](/badges/disobey-2020/) &nbsp;&nbsp; ✅ [CampZone 2019](/badges/campzone-2019/) &nbsp;&nbsp; ✅ [HackerHotel 2019](/badges/hackerhotel-2019/) 
<br> ✅ [Disobey 2019](/badges/disobey-2019/) &nbsp;&nbsp; ✅ [SHA2017](/badges/sha2017/)
{{% /notice %}}

# Reference
| Command          | Parameters                                                            | Description                                                                                                                                                                                                                                                                                            |
| ---------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| flush            | \[flags\]                                                             | Flush the contents of the framebuffer to the display. Optionally you may provide flags (see the table down below)                                                                                                                                                                                      |
| size             | \[window\]                                                            | Get the size (width, height) of the framebuffer or a window as a tuple                                                                                                                                                                                                                                 |
| width            | \[window\]                                                            | Get the width of the framebuffer or a window as an integer                                                                                                                                                                                                                                             |
| height           | \[window\]                                                            | Get the height of the framebuffer or a window as an integer                                                                                                                                                                                                                                            |
| orientation      | \[window\], \[angle\]                                                 | Get or set the orientation of the framebuffer or a window                                                                                                                                                                                                                                              |
| getPixel         | \[window\], x, y                                                      | Get the color of a pixel in the framebuffer or a window                                                                                                                                                                                                                                                |
| drawRaw          | \[window\], x, y, width, height, data                                 | Copy a raw bytes buffer directly to the framebuffer or the current frame of a window. The length of the bytes buffer must match the formula width\*height\*(bitsPerPixel//8). This is a direct copy: color format (bitsPerPixel) must match the specific display of the badge this command is used on. |
| drawPixel        | \[window\], x, y, color                                               | Draw a pixel in the framebuffer or a window                                                                                                                                                                                                                                                            |
| drawFill         | \[window\], color                                                     | Fill the framebuffer or a window                                                                                                                                                                                                                                                                       |
| drawLine         | \[window\], x0, y0, x1, y1, color                                     | Draw a line from (x0, y0) to (x1, y1)                                                                                                                                                                                                                                                                  |
| drawRect         | \[window\], x, y, width, height, filled, color                        | Draw a rectangle at (x, y) with size (width, height). Set the filled parameter to False to draw only the border, or set it to True to draw a filled rectangle.                                                                                                                                         |
| drawCircle       | \[window\], x0, y0, radius, a0, a1, fill, color                       | Draw a circle with center point (x0, y0) with the provided radius from angle a0 to angle a1, optionally filled (boolean)                                                                                                                                                                               |
| drawText         | \[window\], x, y, text, \[color\], \[font\], \[x-scale\], \[y-scale\] | Draw text at (x, y) with a certain color and font. Can be scaled (drawn with rects instead of pixels) in both the x and y direction                                                                                                                                                                    |
| drawPng          | \[window\], x, y, \[data or filename\]                                | Draw a PNG image at (x, y) from either a bytes buffer or a file                                                                                                                                                                                                                                        |
| getTextWidth     | text, \[font\]                                                        | Get the width a string would take if drawn with a certain font                                                                                                                                                                                                                                         |
| getTextHeight    | text, \[font\]                                                        | Get the height a string would take if drawn with a certain font                                                                                                                                                                                                                                        |
| pngInfo          | \[data or filename\]                                                  | Get information about a PNG image                                                                                                                                                                                                                                                                      |
| windowCreate     | name, width, height                                                   |                                                                                                                                                                                                                                                                                                        |
| windowRemove     | name                                                                  |                                                                                                                                                                                                                                                                                                        |
| windowMove       | name, x, y                                                            |                                                                                                                                                                                                                                                                                                        |
| windowResize     | name, width, height                                                   |                                                                                                                                                                                                                                                                                                        |
| windowVisibility | name, \[visible\]                                                     |                                                                                                                                                                                                                                                                                                        |
| windowShow       | name                                                                  |                                                                                                                                                                                                                                                                                                        |
| windowHide       | name                                                                  |                                                                                                                                                                                                                                                                                                        |
| windowFocus      | name                                                                  |                                                                                                                                                                                                                                                                                                        |
| windowList       | \-                                                                    |                                                                                                                                                                                                                                                                                                        |

# Color representation
Colors are always represented in 24-bit from within Python, in the 0xRRGGBB format. This matches HTML/CSS colors which are #RRGGBB as well.

Devices with a smaller colorspace will not actually store the exact color information provided.
For displays with a color depth of less than 24-bit the display driver will automatically mix down the colors to the available color depth.
This means that even if you have a black and white display 0x000000 is black and 0xFFFFFF is white.

# Examples

## Setting one pixel

```
import display
x = 2
y = 3
display.drawPixel(x, y, 0x00FF00)  # Set one pixel to 100% green
display.flush() # Write the contents of the buffer to the display
```

## Drawing a line

```
import display
display.drawFill(0x000000) # Fill the screen with black
display.drawLine(10, 10, 20, 20, 0xFFFFFF) # Draw a white line from (10,10) to (20,20)
display.flush() # Write the contents of the buffer to the display
```

## Drawing text
```
import display
display.drawFill(0x000000) # Fill the screen with black
display.drawText(10, 10, "Hello world!", 0xFFFFFF, "permanentmarker22") # Draw the text "Hello world!" at (10,10) in white with the PermanentMarker font with size 22
display.flush() # Write the contents of the buffer to the display
```

## Drawing a rectangle
```
import display
display.drawFill(0x000000) # Fill the screen with black
display.drawRect(10, 10, 10, 10, False, 0xFFFFFF) # Draw the border of a 10x10 rectangle at (10,10) in white
display.drawRect(30, 30, 10, 10, True, 0xFFFFFF) # Draw a filled 10x10 rectangle at (30,30) in white
display.flush() # Write the contents of the buffer to the display
```

# Known problems
 - Rotation of the contents of windows does not work correctly in combination with rotation of the screen itself
 - There is no method available to list the fonts available on your platform
 - There is no method for providing a custom font
 - There is no anti-aliassing support
