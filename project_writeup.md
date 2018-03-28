# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps. First, I converted the images to HSV color space so that I could apply the color masks for white and yellow colors. 
The second step was to apply the gaussian blur to smoothen out and reduce the noise in the image. After that, a masked image
I got as a result was used as an input to apply the canny edge detection function on. This gave us the edges in the image where there are strongest color gradients in the image. We then apply a region of interest in which we would want to detect the lanes. Step 6 is to get the hough lines. Hough lines are representation of points we got during canny edge detection. Through this function we got the coordinates for the numerous lines and finally the draw_lines() function is used within the hough lines function to remove the unwanted lines and get the two extrapolated solid lines within which we would like 
our vehicle to drive. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first dividing the lines into 
left and right categories by their slopes. Then, I applied slope max and min limits to remove any large deviation that could occur in the averaged slopes.
These coordinates were used in the function fitLine() to the line parameters. The line parameters were appended in deque which like a more efficient and faster list, and were used to calculate the unknown coordinates for the single lines and the slope for them as well.
These were averaged across the 10 frames to stabilize the lines and reduce the jitter. Without this, they appear to move around really fast as the parameters change with every frame.

I have attached how an image goes through the transformation using this pipeline:

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


The algorithm that has been implemented here is a very specific scenario based hard-coded algorithm, which just takes into
picture a specific field of view, the straight or near-to straight road, a normal sunny day with visible lines.
This will not work in different weather conditions (like in Midwest USA!?) when lanes are not visible, curvy roads, 
when there is traffic on the road, during night, during rains which cause the camera to be fooled by reflection.
Also, the parameters chosen for hough lines and region of interest are hard-coded and based on the images provided.

### 3. Suggest possible improvements to your pipeline

Possible improvements that I would suggest is to use algorithm to: 

a. Incorporate curves the plotted lines.

b. Have a variable field of view according to the traffic to know what our driving space can be which also includes other lanes.

c. be able to pick up the lane colors in almost all weather and light conditions 

