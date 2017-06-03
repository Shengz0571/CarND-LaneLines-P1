# **Finding Lane Lines on the Road** 

## Sheng Zhu (shengz0571@gmail.com)

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. My pipeline and draw_lines() function

My pipeline consisted of 5 steps. 
    1. Convert the images to grayscale
    2. Use Canny Edge Detection to find all possible edges (low_threshold = 50, high_threshold = 150)
    3. Set a region (lower half, trapezoid shape) based on the image shape to filter further bacause that is the region where lane liens could be in
    4. Use hough transform to find candidate lines. In my pipeline, I set threshold to 30. This is mainly for the challenge video bacause challenge video has too much noise (shadow edge or clear lines between guard bar and background) I need to deal with. By increasing the threshold value, I cam make sure the lines I found consist of more edges
    5. Finally, combine the lines and the origin image

Beside, In order to draw a single line on the left and right lanes, and try to make them stable (only update x value for each point and slope for each line), I modified the draw_lines() function as follow:
    1. Computer slope for each line, and set ranges to pick out useful candidate lines for left line and right line. The slope range for left line is (-0.85, -0.5), and the slope range for right line is (0.5, 0.85)
    2. Use median point ((x1 + x2) / 2, (y1 + y2) / 2) to represent each line from step one, and computer two median points for final left line and right line
    3. Instead of constantly update two end points of a line, I set the y values for two points as fixed value (image.y and 2 * image.y / 3)
    4. In my pipeline, the left line is represented as green line, and the right line is represented as red line

### 2. Potential shortcomings with my current pipeline


One potential shortcoming is when it deals with the challenge video, espectially when the car is on the light color ground. The lane lines are not very clear, and it has too much noise.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use certain methods to pick out the edges representing the lane line. I tried to use k-means (with TensorFlow packet) to find the group of lines with the least variance, however it took too much time to computer. For a 10s video, it took arond 10 mins to compute

Another potential improvement could be to keep global variables to store lines information (slope and ending points), then when the result of computing a frame changes too much from the last one, then use weight to minimize the change. This is because, I think, the change of the slope or the position of the line are based on only two factors, the speed of the car, and whether the car is in a curve lane.
