# **Finding Lane Lines on the Road**

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of the following steps:

1. Apply a color mask filtering white and yellow (and other bright) areas of the image. This is implemented in the ``color_mask`` function which finds areas of the image within either one of the color ranges  defined by the parameters ``white_mask_range`` and ``yellow_mask_range``.

2. Turn the image grayscale with the standard OpenCV function

3. Apply a gaussian blur filter with the provided function

4. Apply canny edge detection with the provided function. The low threshold has been lowered due to the low contrast in some parts of the challenge.mp4 video.

5. Select region of interest. The ``dim`` parameter is used to scale the points in case the image is not 960 x 540 as is the case with the challenge.mp4 video.

6. Apply Hough transform with the parameters from the lecture.

7. Split lines into ones belonging to the left and right lane boundary respectively. This is done based on the slope of the line - negative slopes belong to the right boundary, positive ones - to the left boundary. Lines with slopes inside [-0.2, 0.2] are thrown out as they are probably an error.

8. For each of the two groups: extrapolate the lines by collecting all of the x-coordinates and all of the y coordinates and fitting a straight line (``deg=1``) through them using ``cv.polyfit``. The calculated values for the fitted line are averaged out so that there are less jitters between frames. Note that this means that the calculated averages have to be reset before each new video is generated. The start and end points of the extrapolated lines are calculated by evaluating the linear function reeturned by ``cv.polyfit`` at the edges of the region of interest.

9. Draw the two extrapolated lines. Note that one line is red and the other one - blue. The code to generate two red lines is commented out (as is the code to overlay the original lines returned by the Hough transformation).

10. Use the provided function ``weighted_img`` to overlay the original image with the two lines.

PS. The actual function to draw lines has not been modified.


### 2. Identify potential shortcomings with your current pipeline

1. Most of the parameters have been set through trial and error. There is no guarantee that they will work for other videos.

2. The pipeline can only generate straight lines. For videos where there's a curve in the road there is no way to cut the line so that it only fits the portion of the lane that is roughly straight. Nor is there a way to generate curves that actually fit the lane.

3. The pipeline struggles to find lines in frames with low contrast.

4. Different lighting conditions will probably not work at all e.g. when there are shadows on some parts of the road and sunlight on others.

### 3. Suggest possible improvements to your pipeline

1. Use a more systematic approach to deriving the algorithm parameters.

2. Try fitting a higher-degree polynomial in step 8 in order to fit curves in the road.


