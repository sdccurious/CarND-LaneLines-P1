# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image0]: ./report_images/solidWhiteRight.jpg "OG"
[image1]: ./report_images/gray_solidWhiteRight.jpg "Grayscale"
[image2]: ./report_images/gaussian_blur_solidWhiteRight.jpg "Gaussian blur"
[image3]: ./report_images/edges_solidWhiteRight.jpg "Edges"
[image4]: ./report_images/region_solidWhiteRight.jpg "Region"
[image5]: ./report_images/regionInverted_solidWhiteRight.jpg "Region inverted"
[image6]: ./report_images/lines_solidWhiteRight.jpg "Lines"
[image7]: ./report_images/linesInverted_solidWhiteRight.jpg "Lines inverted"
[image8]: ./report_images/processed_solidWhiteRight.jpg "Processed"

---

### Reflection

The goal of the pipeline is identify lane markings in the road from color images and videos (example image below).

![alt text][image0]

 Lane markings by nature are made to be easily visible by the human eye/brain.  However getting an algorithm to do the same thing requires special processing.  Lane markings are created to have a high contrast to the rest of the road for visibility.  One way to help detect that contrast is to passing the image through the grayscale() function.

![alt text][image1]

This reduces the amount of color dimensions to process in order to eventually find the edges of the lane markings.  This data reduction can go even farther by applying the gaussian_blur() function.

![alt text][image2]

By smoothing the grays, it will make it easier to find large gradients in the shade of gray between neighboring pixels.  This will help the Canny transform find edges when the smoothed grayscale image is passed through the canny() function.

![alt text][image3]

However there are ends up being a lot of edges that are of little interest in terms of finding lane markings.  The region_of_interest() function was called to cull the amount of data that needs to be processed to find lines.

![alt text][image4]

With the lines filtered down to the region of interest, the Hough transform technique can be applied.  This technique takes the equation of the line from the origin to each pixel in the image.  It then converts this line to the Hesse normal form, and processes the coordinates of *theta* (radians) and *r* (distance from origin).  The hough_lines() functions returns a set of candidate lines which then need to be processed in order to create a single line.

The draw_lines() function needed modification to process the candidate lines.  It helped to think of the image as if it were inverted (so positive Y is up) as shown below.  This made it easier to process slopes and point locations being returned by the hough_lines() function.

![alt text][image5]

The candidate lines in the draw_lines function were each categorized as either left or right lane markings based on their slopes.  In order to decrease the chance non lane marking lines were included, the absolute value of each slope had to fall within a threshold.  Each left/right line was put in a separate collection of left/right slopes and coordinates.  The averages of the slopes and coordinates was used to determine an average slope and location for all the lines.  Then a single line was extrapolated from the top of the image down to 60% of the image height.

![alt text][image7]

With the single line drawn for each side, the lines were superimposed on top of the starting image as shown below.

![alt text][image8]

The settings I had arrived at worked for all the sample images and the standard videos.  It did not work on the challenge video.

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
