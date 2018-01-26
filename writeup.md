# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 
1. First, I converted the images to grayscale
2. I smooth out the image using a gaussian blur
3. Find edges using canny edge detector
4. Mask out everything except the region of interest which contains the lanes
5. Apply hough transform to find line segments on both lanes
6. Draw extrapolated lines on those lanes. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by grouping the line segments into left and right lanes based on their slopes. I remove horizontal lines by filtering out slopes with a threshold. I use an additional filter based on the x position of the line - basically line segments in the left lane have to be less than the mid X point of the image, and conversely the right segments have to be greater than that. Then I fit a line through each set using numpy's polyfit and draw each line using the parameters it returns. 


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming could be what would happen if there is a car within the region of interest. In this case, there will be additional lines that may need to be filtered.

Also, the current code doesn't take care of different shadings on the road which could end up in additional lines that are not lanes.

The identified lane lines are not consistently smooth between video frames. This is due to the way line segments are fit in a line. In some cases, it is due to two edges detected for the same line segment.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to add a non-maximum suppression to the houghlines to remove double edges detected on the same line segment.

Another potential improvement would be to take care of cars or shading on the road within the region of interest.
