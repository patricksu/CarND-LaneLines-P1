# **Finding Lane Lines on the Road** 

## Submitted by: Peng Su 5.27.2017


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./report_images/Issue1.png
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. 1) I converted the images to grayscale. 2) I Gaussian-smoothed the gray image. 3) I detected the edges using Canny. 4) I selected edges only in the region-of-interest. 5) I detected lane lines using Hough Transformation. 6) I combined the lane lines with the original image as output. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function in the following steps.
1) I defined two ploygons for the left lane and right lane, so that only edges in these polygons are considered as lane lines. I did this in order to eliminate a lot of the tree shadow edges (in the challenge video). 
2) Edges in the left polygon with slope in (-1.0, -0.5) is considered as left lane line segment. Edges in the right polygon with slope in (0.5, 1.0) is considered as right lane line segment.
3) The middle points of the left and right lane line segments are used to fit linear-regression lines. The left line and right line are drew into the image. 
4) In the challange video, I noticed a few issues. In the figure below, you can see that only three segments are detected for the left lane line, and a lot of false-positive segments for the right lane line due to shadows. To address these issues, I proposed a moving-average approach, i.e. if lane line slope differs a lot (>0.1) from the previous frame's slope, I used the average of the previous slope and the new slope. Doing this is reasonable since the lane line slopes in the video should not change abruptly. Another change I made is limiting the minimum number of segments to use to fit a linear-regression line. If less than 3 segments are detected for a lane line, I simply used the previous frame's lane line. 
![alt text][image1]
![alt text][image2]


### 2. Identify potential shortcomings with your current pipeline

Scenarios below would cause pipeline failure. 
1) Camera postion and/or angle changed. In this case, the pipeline would fail since the region-of-interest is hard-coded.
2) A different camera. In this case, the edge detection thresholds and Hough transformation parameters would have to change. 
3) City traffic with curbs. The curbs might also be identified as lane lines.
4) Faded lane lines, changing lighting (shadows, darkness, etc.)

The current pipeline has parameters that need to be fine-tuned for different driving scenarios. It is impossible to exhaust every single scenario to get the parameter values, hard-code them, and adjust accordingly. Thus we need a better approach for lane-line detections. 

### 3. Suggest possible improvements to your pipeline

A possible improvement would be running a clustering algorithm to find the lane-line segments in an un-supervised way. Another way of doing this is training a Convolution Neural Network to detect lane lines, using images with annotated lane lines. 
