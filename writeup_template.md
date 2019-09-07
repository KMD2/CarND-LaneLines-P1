# **Finding Lane Lines on the Road** 

## Duoaa Khalifa

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./For_the_markup/grayscale.jpg "Grayscale"
[image2]: ./For_the_markup/Gaussian.jpg "Gaussian Smoothing"
[image3]: ./For_the_markup/Canny.jpg "Canny Transformation"
[image4]: ./For_the_markup/AOI.jpg "Area of Interest"
[image5]: ./For_the_markup/Hough.jpg "Hough Transformation - Final Result"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

To be able to find lane lines on the road, I first tested my pipeline on the six different images provided. The pipeline consisted of 5 steps as described below: 

1- Converted the images to grayscale (from three channels to one channel) to be able to apply Canny transformation on it.

2- Applied Gaussian noise kernel (Gaussian smoothing) on the grayscaled images to help in reducing the noise and receiving strong contrasting boundary lines in the images. I used a kernel size of 7.  

3- Applied Canny transformation to the grayscaled blurred images using the cv2 library. The result was binary images with edges detected on them. I used the values 70 and 150 for the low-threshold and high-threshold respectively to include pixels that are connected with the strong edges only.

4- Highlighted the area of interest in the images (the area that encloses the lanes' lines) by creating a boundary around it in the shape of a  trapezoid. 

5- Finally,  I applied Hough transformation on the edge- detected images to be able to detect the lane lines. After several trials, I ended up setting  the parameters as follows:  rho = 1 to be able to get the maximum resolution pixels, theta = the equivalence of 1 degree in radians, threshold = 35 minimum intersections in the Hough space gird cell, minimum lane width = 5 pixels as the  minimum to create a line and maximum lane gap = 2 pixels as the maximum gap between lines that can be connected to create a line. 


To draw a single line on the left and right lanes, I modified the draw_lines() function by averaging/extrapolating the lines received from the Hough lines image. First, I calculated the slope of each line using the slope equation ((y2-y1)/(x2-x1)), then I calculated the center of each line ([(x1+x2)/2,(y1+y2)/2]). Second, I classified the lines into two categories; right lines belonging to the right lane and left lines belonging to the left lane, I achieved that by checking the slope of each line; if it is positive then it belongs to the right lane and if it is negative then it belongs to the left lane.  Next, I calculated one slope and one center for each category by averaging over the individual lines' values. Then, I defined a line function (y = ms + b) for each category, the (x,y) is the average center, m is the average slope and the intersection (b) was found by rearranging the equation. Now after defining the line equation, the endpoints representing the right and left lanes were calculated by assuming the "y" points to be the start and the end position of the lanes, plugging everything in the equations, we get the "x" points and then we are ready to go.

Below are samples for one image while going through the pipeline steps:

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the lights are dim, or when there are shadows over the lanes from surrounding objects. The developed pipeline wouldn't be able to detect these lines efficiently. 

Another shortcoming could be encountered in a scenario having a car driving right in front of us. The car would interfere with the "area of interest" where the lanes are detected. Parts of the car might be mistakenly detected as potential lines, this will again affect the efficiency of the developed lane detection pipeline.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to convert the images from RGB to HSL (hue, saturation, lightness) color representation instead of grayscale. This will take care of the problem of shading and dim lights conditions.

When the pipeline was applied to the video, it was noticed that the detected lanes were a bit "jumpy", one way to overcome this issue is by average the drawn lanes between a couple of consecutive frames (e.g, the first 10 frames of the video will have the same lanes drawn on them (their average calculated lane) and so on).  
