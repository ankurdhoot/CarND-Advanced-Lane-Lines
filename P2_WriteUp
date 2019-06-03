## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./camera_cal/calibration1.jpg "Distorted"
[image2]: ./camera_cal_undistorted/calibration1.jpg "Undistorted"
[image3]: ./test_images_undistorted/straight_lines1.jpg "Road Transformed"
[image4]: ./test_images_sobel/straight_lines1.jpg "Binary Example"
[image5]: ./test_images_perspective/straight_lines1.jpg "Warp Example"
[image6]: ./test_images_polynomial_fit/straight_lines1.jpg "Fit Visual"
[image7]: ./test_images_curvature/straight_lines1.jpg "Output"
[video1]: ./test_videos_result/project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the second code cell of the IPython notebook located in "P2.ipynb". 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]
![alt_text][image2]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image3]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image. In particular, I used the sobel operation in the x direction and saturation thresholding. Here's an example of my output for this step.

This is `Step 2` in the code.

![alt text][image4]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

To apply the perspective transform, I chose to hardcode the source and destination points in the following manner:

This is `Step 3` in the code.

(Note that this applies to images 1280 x 720 which is our case).

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 750, 450      | 1000, 0       | 
| 1100, 700     | 1000, 700     |
| 250, 700      | 300, 700      |
| 600, 450      | 300, 0        |

An example of the perspective transform:

![alt text][image5]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I found the columns with the most number of 'active pixels' and using the sliding windows technique, fit a second order polynomial to the perspective tranformed result. This is `Step 4` in the code.

![alt text][image6]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

Using the polynomial fit from step 4 (converted to meters), I evaluated the curvature at the bottom of the image using the formula for curvature. The position of the vehicle is found by calculating the distance from the center of the lane to the center of the image. This is `Step 5` in the code.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

This is also part of `Step 5` in the code. I take the polynomial fit and warp it back using the reverse transform of `Step 3`. This is then combined with the original image to produce the following:

![alt text][image7]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./test_videos_result/project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Originally, I had implemented the thresholding using only the Sobel operator in the x direction. This caused yellow lanes to be missed when there was a shadow. To remedy this, I added saturation thresholding which does a decent job detecting the yellow lanes.

The pipeline still fails on the challenge video. In particular, it detects the center of the lane where there appears to a line as the actual lane edge. To make this more robust, I could ensure that the sides of the lane are a sufficient distance apart from each other, and implement some kind of 'memory' so that new frames remember where the old ones left off. That is, use the polynomial fit from the previous frame to guide the fit on the new frame since lanes are generally fairly continuous (or so I hope!).
