# **Finding Lane Lines on the Road** 


[//]: # (Image References)

[image1]: ./steps_images/solidWhiteRight.jpg "solidWhiteCurve"
[image2]: ./steps_images/solidWhiteRight_gray.jpg "greyscale"
[image3]: ./steps_images/solidWhiteRight_gray_blur.jpg "Blurred"
[image4]: ./steps_images/solidWhiteRight_canny.jpg "Edges"
[image5]: ./steps_images/solidWhiteRight_crop.jpg "Cropped"
[image6]: ./test_images_output/solidWhiteRight_output.jpg "solidWhiteCurve Output"
[image7]: ./steps_images/slopevar.jpg "draw_lanes() figure"


---

### Goals

The goals of this project are the following:
* Make a pipeline that finds lane lines on the road
* Test the pipeline on dashboard images
* Apply the pipeline on dashboard videos
* Improve on the pipeline and test with the challenge video
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

* Step 5: I run a hough transform  to define the lines of edges and then draw a single line for each side on the lane.
![alt text][image6]


In these steps, I used the thersholds suggested in the Quiz solutions. The main modification to the code provided was in the draw\_lines() function in order to draw a single line per lane side.

**How I modified the draw\_lines() function:**
  
 I removed the line drawing from the loop over all the lines and replacing it by the averaging of both the slope of the lines and the positions of their center in order to draw online one line per side using the average slope and the center of center of lines. 
 
 ![alt text][image7]
 
 For each line betwen points (x1,y1) and (x2,y2), the slope a and the length d of the line are calculated: (see figure above for a visual representation of the variables)
>  a= (y2-y1)/(x2-x1)
>  d= math.hypot(y1-y2,x1-x2)
 
 The slope is used to classify the line as part of the left lane line or the right lane line. And for each side, the sum of the slopes and the x and y positions of the centers of the lines are calculated. An example from the right side is:
>  if a>0.25 :
>       i_right += d
>       a_right += a  * d
>       y_right += y1 * d
>       x_right += x1 * d

Note: The having a bigger than 0.25 or smaller than -0.25 instead of 0 helps remove lines perpendicular to the road from being used in the calculating the slope and position of the lane lines.

 Multipling by the length of the line and adding it to i\_right is so that at the end, we can obtain a line-length-weighted average of slopes and the positions of the centers of the lines:
> if i_right > 0: 
>        a_right /= i_right
>        y_right /= i_right
>        x_right /= i_right
>        b_right = y_right - a_right * x_right
>        cv2.line(img, (int((imshape[0]-b_right)/a_right), imshape[0]), (int((y_min-b_right)/a_right),y_min), color, thickness)

The if statement here makes sure there is at least one line detected as a right or left line so that we don't devide by zero.

Finally, to draw the line, I solved for the x positions of the intersection of the calculated line with the bottom border of the image (imshape[0]) and with the line y= y\_min the upper end of the detected line that is closest to the top (and smallest y). 

Please refer to the jupyter notebook P1.ipynb for the full script. Note that I haven't uploaded the source images and videos so running the cells on the notebook again would not work.


### 2. Shortcomings of the pipeline described in 1.

As I tried the above pipeline on the challenge video, I discovered that the lines detected were eclectic because they were detecting edges in the asphalt. These edges are the edges between area of darker and lighter grey color probably due to the way the road was paved. Trees and shadows created more edges that confused the pipeline too. Please refer to the Jupyter notebook for the example video.


### 3. Improvements to your pipeline

To remedy the shortcoming described in 2., I had to teach the pipeline to see in colors, in yellow and white particularly.

For isolating the white part of the image, I used cv2.inRange to select pixels having blue, red, and green values greater than 200.
> image_white = cv2.inRange(image,np.array([200,200,200]),np.array([255,255,255]))

For the yellow part of the image, I had to first convert the image to the HSV representation of the colorspace. 
>hsv = cv2.cvtColor(image, cv2.COLOR_RGB2HSV)

Then, I converted the yellow color values in RGB to HSV to find the "Hue" value of yellow which is 30. I grabbed the pixels of the image that have Hue values betwen 20 and 40 and with at least a value of 100 for their "Chroma" and "Value".
>image_yellow= cv2.inRange(hsv, np.array([20,100,100]),np.array([40,255,255])) 

I then run the canny detection algorithm seperately on each of the seperated images and combined them before cropping and drawing the lanes lines. This improvement worked as is shown in the output video on the jupyter notebook. 


### 4. Suggest possible improvement to your pipeline

One shortcoming is that the right line lane on the challenge video is not continuous throughout the video. A possible improvement on this is to have the pipeline "remember" the last position of the lane so that it transitions smoothly between frames. This can be done by averaging new slopes and positions of the line of each new frame with the previous few frames.

Another shortcoming is the fact that the cropping step of the pipeline relies on the input of arbitrary points to delimit the area of interest. This area would be different from a system to another depending on the angle of the dashboard camera, the grade of the road, and the presence of obstacles like cars on the road. This step could instead be automated using a mask that looks for the parts of the image that have the usual colors of a paved road. 

### Goals check

The goals of this project were all accomplished:
* DONE: Make a pipeline that finds lane lines on the road
* DONE: Test the pipeline on dashboard images
* DONE: Apply the pipeline on dashboard videos
* DONE: Improve on the pipeline and test with the challenge video
* DONE: Suggest more improvements
* DONE: Reflect on my work in this written report

