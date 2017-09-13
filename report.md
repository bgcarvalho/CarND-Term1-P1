# **Finding Lane Lines on the Road** 

## Term 1 - Project 1


**Introduction**

The goal of this project is to create pipeline that finds lane 
lines on the road in images and in video stream, and finally present
the work in a written a report.


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

[image01]: ./output_images/2_1_grayscale.png
[image02]: ./output_images/2_2_gradient.png
[image03]: ./output_images/2_3_masked.png
[image04]: ./output_images/2_4_mask_in_red.png
[image05]: ./output_images/2_5_mask_red_continous.png
[image06]: ./output_images/2_6_final.png


### Processing input - The pipeline

The input input images and video streams consists of pictures of the a highway
from the perspective of a camera in a car facing forward.

Detecting and placings a layer over the lane lines consists of 6 steps:

1. Convert to grayscale. This step removes all colors, so we threat all lane
lines the same way.

![Input image 2 in grayscale][image01]

2. Apply a Gaussian blur to soften or remove noise and spurious gradients.

3. Calculate Canny gradients in threshold limit from 50 to 150.

![Input image 2 gradients][image02]

4. Calculate vertices and mask the gradient image. This step selects only the 
part of interest in the image. It was assumed as hypothesis that lanes always
start near the center of the image and the lines go down to the bottom border.
So it is proposed the vertices as a trapezoid adjusted by values `deltax1`,
`deltax2` and `deltay`.

![Input image 2 masked][image03]

5. Calculate Hough lines, converting points to polar coordinates. The parameters
`threshold`, `min_line_length` and `max_line_gap` have decisive role on
detecting lines. Values 3, 15, and 5, respectively, were found to fit best
the examples. The resulting lines from the Hough transform can be plotted 
directly or processed further.
The modified version of `draw_lines` was called `draw_left_right_lines`
calculates the average of coeficients `a` and `b` for both lines 
(`y = a*x + b` left and right). The point of intersection is also checked to 
avoid the lines spreading throughout the image.

![Input image 2 masked in red][image05]

6. Add lane lines over the original image.

![Input image 2 final][image06]


### Pipeline shortcomings

The pipeline implemented for the work may not work for different environment
conditions. It is probable that in rainy or at night the algorithm fails.
Variation in lightining and trepidation are important aspects of edge detection.
A different position of the camera on the car and its alignment with the road
require modifications in the code.

### Possible improvements

From the video stream output is possible to see great variation in the results
from frame to frame. One possible improvement would be to compare one frame
to the previous, smothing the transintion in slope for each calcultion.

Another improvement possible is apply a higher contrast right after the 
grayscale convertion.



