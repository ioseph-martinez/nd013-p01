#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./outputsolidYellowCurve.jpg "lane detection"

---

### Reflection

###1. Pipeline Description

My pipeline consisted of 5 steps.
- RGB to Grayscale
- Gaussian Filtering (kernel size 5)
- Canny detection (parameters 50 and 150)
- ROI: here I actually use two ROIs one for left and one for right. Horizon and center is important here.
- Two separate Hough Lines Detection. Tuned the parameters for best detection in the videos: (rho= 1, theta = 1deg, Th = 8, Min Lenght = 7, Max Gap = 5)

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by using a statitical method (least squares) to obtain the best fitted line for the points detected from the hough line algorithm.
Our Hough Lines returns a mask of the image with only the lines pixels detected. These can be used as points and therefore least squares can be applied. Numpy function nonzero will return a list of points of the non zero pixels that can be used with least squares.
The resulting line is a good average of each side of the lanes.

Below an example image:

![alt text][image1]


###2. Identify potential shortcomings with your current pipeline

Potential shortcomings:
- If the car switches lanes left and right points could mix together in a single ROI (i.e. right points in left roi)
- Some roads (like in Las Vegas) don't have lane lines, just small reflectors squares. These "lines" would be hard to detect. Also roads that don't have markers at all, just the division from asphalt and terrain.
- Contrast changes, light changes can affect the performance of Canny and Hough Lines.
- Cars to close to the car front or on the sides would be hard to detect because lines would be hidden from the camera
- Pedestrian Crossings would add various similar lines that could confuse from the actual lane lines.

###3. Suggest possible improvements to your pipeline

Various improvements could be applied:
- Lane lines join in the apex at the horizon. Lines not doing this could be filtered.
- Lines with certain angle, for example 0 to 15 or 20 are unlikely to be lane markings
- Yellow over White is hard to detect, applying more weight to the blue channel can help.
- Corner detection could be applied to detect other type of markings like rectangular reflectors on some roads
- Temporal tracking and kalman filters could be applied given that we know the physics/mechanics of the car
- ROIs can be dynamically adjusted based the latest detection and track them if the car is changing lanes
- When doing least squares, filtering can be done on outliers points.
