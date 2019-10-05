# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteRight.jpg
[image2]: ./test_images_output/solidYellowCurve.jpg
[image3]: ./test_images_output/solidWhiteCurve.jpg
[image4]: ./test_images_output/solidYellowCurve2.jpg
[image5]: ./test_images_output/whiteCarLaneSwitch.jpg
[image6]: ./test_images_output/edges_with_areaofint.jpeg

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the general steps for canny edge detection and then calculating the hough transform.
Below steps show the details of my pipeline.

1. Canny edge detection - 
I selected the parameters for edge detection by tuning the variables such that the lanes in the challenge.mp4 are clearly visible. Controlling gaussian blur was key since the lines are unrecognizable due to the color of the concrete section.

2. Polygon mask - 
Created a polygon mask to include the area of interest. Since the image resolution of the video did not match the test images, I used percentage of maximum size to control the size of the polygon.

![alt text][image6]
Edge detection and area of interest mask on challenge.mp4


3. Hough Transform - 
Hough transform parameters were tuned to get the lan lines desired and to reject the cracks in the center being mistaken for lanes. A line length lower than 20 and threshold less than 30 would cause random cracks to be detected as lane lines.

4. Extending lanes - 
Since the lane lines detected were broken segment, some processing was necessary to ensure that a single line is used for each lane. I calculated the slopes of the line and if they fall in the range of 0.5 to 0.8, they would be a lane line. Then I extended each vertex to the bottom and center of the screen.
Additionally I also calculated the length of the line detected. This can be used to filter out the uncertainities of the smaller lane segments.

![alt text][image5]
White car lane switch

5. Single line selection - 
To ensure just one lane line is selected, I used flags for lines each for positve and negative slope. This ensures just one instance of each lane line is used.

![alt text][image1]
Solid White Right

![alt text][image3]
Solid White Curve
![alt text][image2]
Solid Yellow Curve
![alt text][image4]
Solid Yellow Curve 2




### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be error arising when the concrete section is too long and has cracks in the center. This would cause a line to be drawn in the center of the lane. 

Additionaly, the lane lines were very shaky which can affect control algorithms downstream.
 
Another shortcoming could be switching lanes. Due to a number of conditional parameters being set, the code would not adapt well when the lane lines have a different slope.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to boost the yellow and the white colors in the image manually and then convert to grayscale. Setting these colors strictly to 255 will ensure a better gradient and would be easier to pick for the Canny edge detection.

To reduce the vibration of the detected lane lines, check the lengths of the lane segments and select the segment with maximum length to draw a lane rather than selecting the first one which falls in the range.

We can also introduce some Gaussian averaging techniques for non-solid lane lines so that they are more stable.
