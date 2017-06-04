# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[image1]: ./test_images_out/solidWhiteCurve.jpg "Example"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

In order to detect the lanes on the road, the pipeline consist of the following steps:

* Find the edges on the image:

In order to detect the edges, the image is first converted grayscale and a gaussian filter is applied.
A Canny edge detection filter is then applied to identify the edges.

* Find the segments formed by the detected edges:

While the canny filter is applied to the whole input image, we know the approximate location where to look for the lanes.
Then before applying the Hough transform to find the segments, we mask the area where we know the lanes will not be.
The result is a list of segments detected in the region of interest.

* Extrapolate to find the position of the left and right lanes:

We want only one line for each of the left and right lane.

To compute them from the list of segments:
- we discard all the segments that do not have the expected slope to eliminate segments that are not from the lanes,
- we separate the segments in two buckets based on the sign of their slope, one for the left lane and one for the right lane,
- we store one point coordinate, the slope and the length of the segment for each candidate,
- we average the coordinates and the slopes for each of the bucket using the length of the segment as a weight to get a single point and slope for each bucket,
- we draw the line passing by the point with the given slope from the top to the bottom of the region of interest to represent the line.

* Output:

Processed images and videos are located in the test_images_out folder and test_videos_output folder respectively.

![alt text][image1]

### 2. Identify potential shortcomings with your current pipeline

- The pipeline detects straight lines only and would not work on curves,
- The region of interest does not account for the heading of the car so the the detection would not work when the car is turning,
- The shape of the region of interest is simple then shadows / cars / markings on the road in front of the car might trigger false positive,
- The piepline is tuned and tested in daylight conditions only.

### 3. Suggest possible improvements to your pipeline

- The pipeline run the edge and line detections on the full extend of the image while lanes are only detected in the region of interest. The perfomance of the algo could be greatly improved by running the pipeline on the bouding box of the region of interest only (the smallest rectangular area that contain the region of interest),
- The pipeline should account for history to make more accurate prediction: once we know where the lanes are on a video frame we could further restrict where to look for the lanes on the next frame. Using history could improve both the performance and the accuracy of the output.