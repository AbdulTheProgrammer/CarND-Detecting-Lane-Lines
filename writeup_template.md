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

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied a gaussian blur with a kernel size of 7. Then the resultant image was run through a canny edge function to extract the most prominent edges from the image. The output of this operation was then masked with polygon image to specify a rough region of interest. The region was shaped to be a trapezoid to best isolate the lane lines and was defined using four vertices. Then a hough transform is performed on the output image to extract the lane lines of interest. The draw_lines function was altered in order to extrapolate these lane lines. This operation was conducted by iterating through each of the lines produced by the hough transform and determinging the average slopes and y-intercepts for the left lane and right lane respectively. The method for determining if the line belonged to the left lane or right lane was based off the fact that lines with a negative slope belonged to the left lane and lines with a positive slope belonged to the right lane. Using the average y-intercepts and slopes, the left and right lane lines were extrapolated using the fact that the lines started at a y position of 0 and extended to a maximum of 315 pixels. Finally the last step of the pipeline was to overlay the lines onto the image in a weighted manner to produce the fully processed image. 


If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when a hill or other uneven terrain is encountered. This is a problem as  currently the image processing pipeline uses a static region of interest to extract the lane lines, which would have dynamically change to address the changing terrain. This also means sharp turns would also pose a problem. 

Another shortcoming could be that this pipeline also does not handle shadows on the road very well or poor lighting conditions. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to choose a more complex polygon for the region of interest to better capture just the lane lines and less of the center region. 

Another potential improvement could be to use the dilate function in openCV after the canny edge detection process to further remove any noise or unwanted lines from the image before the hough transform is conducted
