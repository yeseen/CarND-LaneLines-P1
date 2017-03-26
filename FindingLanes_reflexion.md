#**Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Test the pipeline on dashboard images
* Apply the pipeline on dashboard videos
* Improve on the pipeline to work with the challenge video
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"


---

### Reflection
https://review.udacity.com/#!/rubrics/322/view

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I apply a gaussian blur with a kernel size of 5. A canny edge detection algorith is applied next on the blurred immaged with the minimum and maximum thresholds set to 50 and 150 respectively. The result is cropped to a region of interest, and I run it through a hough transform algorithm to define the lines of edges. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by removing line drawing from the loop over all the lines and replacing it by the avergaing of both the slope of the lines and the positions of their center. 
For each line, the slope and the length of the line are calculated. The slope is used to classify the line as part of the left line or the right line. And for each side, the sum of the slopes and x and y positions of the centers of the lines are calculated. Each of these quantities are multiplied by the length of the line so that we obtain a line-length-weighted average of slopes and the positions of the centers of the lines. 

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]
https://en.wikipedia.org/wiki/HSL_and_HSV#Hue_and_chroma
http://docs.opencv.org/3.2.0/df/d9d/tutorial_py_colorspaces.html
http://colorizer.org/
http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_colorspaces/py_colorspaces.html

###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...