#**Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---
### Output
Both the Image output and the Video output are in the folders named appropriately.  I have completed the assignment from that perspective.

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My code first calls process_image(image) which calls draw_lanes(input_image) which is the real function that processes the image. 

1. First I convert the image to grayscale

2. Then I apply gaussian blurring with kernel=3

3. Then I create a Canny image with low_threshold = 50 and high_threshold = 150

4. Then I define the trapezoid which is the area of interest

5. Then I use the hough_lines function with rho=2, theta=1 rad, threshold = 20, min_line_len = 40, max_line_gap=10

6. Then the core function that hough_lines uses is draw_lines() - described below

In order to draw a single line on the left and right lanes, I modified the draw_lines() function as follows:

1. For every line I find the slope of the line m and x-intercept named 'c'. 

2. Then if m is negative it is the left lane, if m is positive it is the right lane

3. Then for drawing the left lane, I average over all the m=slopes and the c=intercepts. This is the weighted average where weight = square the of the length of the segment.  I want to give more weightage to longer segments, hence I didn't use the segment length, but the square of the length of the segment

4. I draw the actual Hough Lines in Blue

5. And I draw the Left and Right lanes in Red. I have made them thicker. The lane segments go from the bottom of the image to the highest point on the image as identified by the Hough Lines.  I think this is important because we shouldn't just draw the lane height to a fixed length.

All images are included in the directory attached.

###2. Potential shortcomings with your current pipeline & Suggestions

1. I discovered that the algorithm to aggregate & average lane segments is flawed because the intercepts and the slopes could be NaNs. This has to be avoided. So I will work on converting it into the rho, theta parameter format next.  This will also ensure that lines with bigger slopes don't get a higher weight.

2. We haven't yet figured out a way to curve the lanes.  This is important as the Car has to respond to upcoming curves. 

3. Also lanes marked in this pipeline are only in the area of interest.  This will fail during sharp turns, cities, or when the car is off course and needs to course correct. 

