# **Finding Lane Lines on the Road** 


[//]: # (Image References)

[image1]: ./steps_images/solidWhiteRight.jpg "solidWhiteCurve"
[image2]: ./steps_images/solidWhiteRight_gray.jpg "greyscale"
[image3]: ./steps_images/solidWhiteRight_gray_blur.jpg "Blurred"
[image4]: ./steps_images/solidWhiteRight_canny.jpg "Edges"
[image5]: ./steps_images/solidWhiteRight_crop.jpg "Cropped"
[image6]: ./output_images/solidWhiteRight_output.jpg "solidWhiteCurve Output"




---

### Goals

The goals of this project are the following:
* Make a pipeline that finds lane lines on the road
* Test the pipeline on dashboard images
* Apply the pipeline on dashboard videos
* Improve on the pipeline to work with the challenge video
* Suggest more improvements
* Reflect on my work in this written report

### 1. Pipeline description:
I will give examples of the pipeline steps using this image:
![alt text][image1]

My pipeline consisted of 5 steps:
* Step 1: I convert the images to grayscale
![alt text][image2]

* Step 2: I apply a gaussian blur with a kernel size of 5. 
![alt text][image3]

* Step 3: I apply a canny edge detection algorithm with the minimum and maximum thresholds set to 50 and 150 respectively. 
![alt text][image4]

* Step 4: I crop the result a region of interest that removes the detected edges that are grossly off the road
![alt text][image5]

* Step 5: I run a hough transform  to define the lines of edges and then draw a signle line for each side on the lane.

I used the thershold suggested in the Quiz solutions for the functions used in these steps. The main modification to the code provided was in the draw\_lines() function in order to draw a single line per lane side.

**How I modified the draw\_lines() function:**

MAKE FIGURE HERE!! 

The figure above summarizes the modification so that the rest of the text in this paragraph doesn't need to be read unless the figure doesn't convey the message. I removed the line drawing from the loop over all the lines and replacing it by the avergaing of both the slope of the lines and the positions of their center. For each line, the slope and the length of the line are calculated. The slope is used to classify the line as part of the left line or the right line. And for each side, the sum of the slopes and the x and y positions of the centers of the lines are calculated. Then, they are multiplied by the length of the line so that at the end, we obtain a line-length-weighted average of slopes and the positions of the centers of the lines. 





### 2. Shortcomings with the pipeline described in 1.



### 3. Improvements to your pipeline
https://en.wikipedia.org/wiki/HSL_and_HSV#Hue_and_chroma
http://docs.opencv.org/3.2.0/df/d9d/tutorial_py_colorspaces.html
http://colorizer.org/
http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_colorspaces/py_colorspaces.html

### 4. Suggest possible improvement to your pipeline

- automate cropping 


A possible improvement would be to ...

Another potential improvement could be to ...

