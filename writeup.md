# **Finding Lane Lines on the Road** 

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

My pipeline for line detection in a single image consists of 6 steps. 
1. I converted the image to grayscale.
2. I use a gaussian blur filter to smooth out areas where individual pixels could lead to misdetections later.
3. I use the Canny function of OpenCV to detect the edges in the image.
4. I select a "region of interest" around the lane that I want to detect the lines from.
5. I use the Hough transform to find the lines in the region of interest. The parameters are tuned so that I am good with the results.
6. The lines found with the Hough transform are overlaid with the original image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function.
1. As it was suggested in the video on Youtube, I calculate the average slope and center points of the lines detected with the Hough transform.
2. I actually only "allow" slopes within a certain range for the left lane and the right lane.
3. Based on the average slopes and center points for the left and right lane, I calculate points that define the extrapolated lines from the bottom of the image where the lane starts to be seen, to the end of the region of interest.
4. With the calculated start and end points for the left and right extrapolated lines, I then simply call the cv2.line() function with these points to draw the two extrapolated lines.
5. Again the overlay with the original image works with the weighted_img() function.

Since the improved draw_lines() function again works on single images, for the video simply each frame image is analysed and the lines are drawn into it and in the end the images are again assembled into a video format.


### 2. Identify potential shortcomings with your current pipeline


The potential shortcomings can be clearly seen in the optional challenge video:
1. The solution doesn't work well with curved lines.
2. The solution doesn't work well with other cars in the picture whose edge can be mistakingly seen as a line with falling into the same slope range as one of the lanes.


### 3. Suggest possible improvements to your pipeline

Possible improvements could be:
1. Tune the slope regions well to the likely slopes of lanes based on camera position etc. I tried to optimize and this helped to at least recognized the lane in the challenge video for a few seconds until an execption occurs.
2. For the selection of lines a color filtering for white or yellow lanes could be used.
3. For curved lines maybe it is better to fit curves then lines. Yet I don't know how this would be done.
