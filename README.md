This is my submission for the Udacity Self-Driving Car Nanodegree Lane Detection Project. You can find my code in this [Jupyter Notebook](https://github.com/pszczesnowicz/SDCND-P1-LaneDetection/blob/master/LaneDetection.ipynb). The goal of this project was to implement a computer vision pipeline that would detect and highlight lane markers. The following is a brief walk-through my code.

# Summary

My pipeline takes a video input and outputs it with the lane markers highlighted.

<img src="https://raw.githubusercontent.com/pszczesnowicz/SDCND-P1-LaneDetection/master/readme_images/initial.jpg" width="400"> <img src="https://raw.githubusercontent.com/pszczesnowicz/SDCND-P1-LaneDetection/master/readme_images/final.jpg" width="400">

# Pipeline

1. First, my pipeline converts the video frame to a grayscale image.

	<img src="https://raw.githubusercontent.com/pszczesnowicz/SDCND-P1-LaneDetection/master/readme_images/grayscale.jpg" width="400">

2. Then, to reduce noise and smooth the image, a Gaussian blur is applied to the grayscale image.

	<img src="https://raw.githubusercontent.com/pszczesnowicz/SDCND-P1-LaneDetection/master/readme_images/gaussian_blur.jpg" width="400">

3. The blurred image is then fed through a Canny edge detection algorithm which outputs an image containing only the edges.

	<img src="https://raw.githubusercontent.com/pszczesnowicz/SDCND-P1-LaneDetection/master/readme_images/canny_transform.jpg" width="400">

4. To further reduce noise, everything outside of a region where we would expect to find the lane markers is masked.

	<img src="https://raw.githubusercontent.com/pszczesnowicz/SDCND-P1-LaneDetection/master/readme_images/mask.jpg" width="400">

5. A Hough transform is then applied to the masked image. The output is an array of coordinate points that correspond to lines in the masked image. The `hough_lines` function separates the lines based on their slope, i.e. left and right lane markers, and ignores lines with slopes outside of a specified range, e.g. horizontal and vertical lines. The following operations are done separately for the left and right sets of coordinates.

6. These coordinate points are then passed to a function which does the following:

	1. calculates the lengths of the lines defined by the coordinate points,

	2. calculates a weighted average of these lines using their lengths as the weights, and

	3. calculates the slope and intercept coefficients of the average line:

		1. If this is the first frame that is being fed through the pipeline, then the coefficients are stored in a global variable.

		2. If not, then the coefficients for the current frame are averaged with those stored in the global variable. The global variable is then updated with the new average coefficients. This reduces the jumpiness of the lane marker highlights.
	
7. Finally, the lane markers are highlighted on the original image using the average coefficients.

# Results

My pipeline had little difficulty identifying the lane markers in the first and second video. The third video presented a challenge in the form of different road surfaces and shadows.

[<img src="https://raw.githubusercontent.com/pszczesnowicz/SDCND-P1-LaneDetection/master/readme_images/white_lane_line.jpg" width="640">](https://youtu.be/mFpdIEN4TEY "Click to watch")

[<img src="https://raw.githubusercontent.com/pszczesnowicz/SDCND-P1-LaneDetection/master/readme_images/yellow_lane_line.jpg" width="640">](https://youtu.be/GZr78FPJE3Y "Click to watch")

[<img src="https://raw.githubusercontent.com/pszczesnowicz/SDCND-P1-LaneDetection/master/readme_images/challenge.jpg" width="640">](https://youtu.be/vL-lKtjUu_o "Click to watch")

# Conclusion

This pipeline was set up for videos that were taken in good weather and with lane markers that contrast well with the road surface. Because of its static coefficients, it does not perform as well when applied to videos with different conditions.

To reduce noise, the pipeline ignores lines with slopes less than 0.5 and greater than 2, but this would also ignore pedestrian crossings, intersections, etc..

This project is succeeded by the [Advanced Lane Detection and Vehicle Tracking Project](https://pszczesnowicz.github.io/SDC-P4-5-Advanced-Lane-Detection-Vehicle-Tracking/) which solves these issues.

# References

[Udacity Self-Driving Car ND](http://www.udacity.com/drive)

[Udacity Self-Driving Car ND - Term 1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit)

[Udacity Self-Driving Car ND - Lane Finding Project](https://github.com/udacity/CarND-LaneLines-P1)
