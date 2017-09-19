# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.
My pipeline is composed of several algorithems and functions, apply them to an image, and produce an annotated image that shows where a lane on a road would be.

My pipeline implementation is summarized in the following steps:
- Applying a color mask by converting RGB to HSV
- filter Yellow and white lines from HSV plane
- Apply Gaussian blur on the resulted yellow and white mask 
- Perform edge detection
- Define region of interest to search for lane lines
- Using the Hough transform to find line segments
- Consolidate and Extrapolate the lane from the line segments and apply to original image

## Color Masking

### HSV Color Space
Using `cv2.cvtColor`, we can convert RGB image into HSV color space.

![alt text][image1]
### Build the filter to apply on image to get white and yellow lines.
Using cv2.inRange build the filter to seperate yellow and white lines.

## Using Canny Edge Detection and Gaussian blur
I used gaussian blur to smooth out the edges by reducing the noise.The important parameter is kernel_size, bigger kernel_size means more blurred image.
I used Canny edge detection to identify straight lines by detecting the edges from the given image.The Canny edge function takes a high and low threshold as a parameters — (canny_high_threshold - minimum difference in intensity to establish an edge and canny_low_threshold - to form a contiguous extension of an established edge)
- use opencv function `cv2.GaussianBlur` to remove noise from the edges.
- use opencv function `cv2.Canny` to find edges

To identifing the right values for the parameters to produce desired output,I first set the `canny_low_threshold` to zero and then adjust the `canny_high_threshold`. If `canny_high_threshold` is too high, you find no edges and if it is too low then you will see too many edges.later adjust the `canny_low_threshold` to discard the noise connected to the strong edges.

## Define Region of Interest (ROI) Selection
Only keeps the region of the image defined by the polygon formed from `vertices` and exclude outside region of interest by setting it to black. 

## Using the Hough transform to find line segments
The hough transform that actually finds line segments in the image and provides the most information of where the lanes lines could be. 
It takes a resolution for line position and orientation, a minimum number of points to establish a line, the minimum length of a line, and the maximum gap between points allowed for a line as parameters.Tuning the parameters was crucial part of the pipeline.

## Consolidate and Extrapolate the lane
Once we have the line segments produced by the hough transform, we can find a line that would be suitable for annotating and extrapolating the image.

### 2. Identify potential shortcomings with your current pipeline
The challenge video exposed a couple flaws with my pipeline:

    Highly sensitive to image contrast
    pipeline stumbles during curve path


### 3. Suggest possible improvements to your pipeline

Identify the right parameters for hough transform and edge detection.Way to identify at run-time.
