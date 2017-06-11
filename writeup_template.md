# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[hough]: ./test_images_output/hough.png "Hough Transform"

[mask]: ./test_images_output/masked.png "Combined Yellow and White Color Mask"


[interest]: ./test_images_output/interest.png "Region of Interest Mask"

[edges]: ./test_images_output/edges.png "Canny Edge Transform"

[weight]: ./test_images_output/weight.png "Final Output Image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 simple steps. 

1. I converted the image to the HSL and HSV color spaces. A conversion to the HSV color space was conducted to create a yellow hue mask to identify the yellow lane line. I avoided using the RGB color space to make the mask as accurately extracting regions of yellow proved to be too challenging due to variations in lighting during the video stream, which would . Moreover, the conversion to the HLS color space was used in order to more easily obtain the upper and lower threshold values for the white color mask. Both these color masks were combined to retain only the yellow and white objects in the original image. 


2. I applied a gaussian blur with a kernel size of 7. 

3. The resultant image was run through a canny edge function to extract the most prominent edges from the image. 

4. The output of this operation was then masked with polygon image to specify a rough region of interest. The region was shaped to be a trapezoid to best isolate the lane lines and was defined using four vertices. 

5. A hough transform was then performed on the output image to extract the lines of interest from the Canny edge transform. 

6. The lines from the Hough Transform were then provided as input to my custom draw_lines function, which was designed to filter and extrapolate the lane lines. This operation was conducted by iterating through each of the lines produced by the hough transform and determing whether they belonged to the left lane line or right lane line based on the slope. This step also filtered out some of the lines based on slope. Once the lines were catagorized, a linear regression was performed using the polyfit function on both groups of lines and the resultant parameters were used to extrapolate the two lane lines to the required length. 

7. Finally the last step of the pipeline was to overlay the lines onto the image in a weighted manner to produce the fully processed image. 

**Combined Yellow and White Color Mask Applied
![alt text][mask]


**Canny Edge Transform
![alt text][edges]

**Region of Interest Mask Applied
![alt text][interest]

**Hough Transform
![alt text][hough]

**Final Output Image
![alt text][weight]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming of this pipeline is that it cannot handle hills or other uneven terrain. This is a problem as  currently the image processing pipeline uses a static region of interest to extract the lane lines, which would dynamically have to change to address the changing terrain. This also means sharp turns or curved sections of highway would pose a problem. 

Another shortcoming could be that this pipeline also does not handle shadows on the road very well or poor lighting conditions often present at night. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to choose a more complex polygon(s) for the region of interest to better capture just the lane lines and less of the center region. 

Another improvement could be an strategy could be the use of a higher degree polynominal to fit the data points produced from the hough transform. This idea would address the presence of curved roads in the real world. Nevertheless, I imagine this improvement would greatly increase the complexity of the project and more testing be necessary to choose the best polynominal for different roads.
