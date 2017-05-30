## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.  -- COMPLETED
* Apply a distortion correction to raw images. -- COMPLETED
* Use color transforms, gradients, etc., to create a thresholded binary image.  -- COMPLETED
* Apply a perspective transform to rectify binary image ("birds-eye view").  -- COMPLETED
* Detect lane pixels and fit to find the lane boundary.  -- COMPLETED
* Determine the curvature of the lane and vehicle position with respect to center. -- COMPLETED
* Warp the detected lane boundaries back onto the original image.  -- COMPLETED
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.  -- COMPLETED
* Projected video with land boundaries and numerical estimate of curvature and vehicle position -- COMPLETED
* (Optional) Challenge Video with land boundaries and curvature -- NOT COMPLETED

[//]: # (Image References)

[image1]: ./output_images/distorted_corrected_calibration1.png "Undistorted Chess Board"
[image2]: ./output_images/undistorted_straight_line1.png "Undistorted straight_line1.jpg"
[image3]: ./output_images/sobelx_thresh.png "Threshold X Gradient"
[image4]: ./output_images/sobely_thresh.png "Threshold y Gradient"
[image5]: ./output_images/magnitude_thresh.png "Magnitude of Gradient threshold"
[image6]: ./output_images/direction_thresh.png "Direction of Gradient threshold"
[image7]: ./output_images/combined_thresh_binary.png "SobelX, Magnitude and Direction Combined Gradient threshold"
[image8]: ./output_images/s-channel.png "S Channel Binary"
[image9]: ./output_images/s-sobelx.png "S Color Channel and SobelX Gradient threshold"
[image10]: ./output_images/s-combined.png "S Color Channel and Combined Gradient threshold"
[image11]: ./output_images/warped_straight_line1.png "Perspective Transform of Straight_line1.jpg"
[image12]: ./output_images/warped_binary_test1.png "Warped Binary for test1.jpg"
[image13]: ./output_images/warped_binary_test2.png "Warped Binary for test2.jpg"
[image14]: ./output_images/warped_binary_test3.png "Warped Binary for test3.jpg"
[image15]: ./output_images/warped_binary_test4.png "Warped Binary for test4.jpg"
[image16]: ./output_images/warped_binary_test5.png "Warped Binary for test5.jpg"
[image17]: ./output_images/warped_binary_test6.png "Warped Binary for test6.jpg"
[image18]: ./output_images/lane_location_straight_line1.png "Locate the lane lines and fit a ploynomial"
[image19]: ./output_images/lane_location_next_stright_line1.png "Locate the lane lines and fit a polynomial (second image)"
[image20]: ./output_images/laneline_binary_test1.png "Lane Lines for test1.jpg"
[image21]: ./output_images/laneline_binary_test2.png "Lane Lines for test2.jpg"
[image22]: ./output_images/laneline_binary_test3.png "Lane Lines for test3.jpg"
[image23]: ./output_images/laneline_binary_test4.png "Lane Lines for test4.jpg"
[image24]: ./output_images/laneline_binary_test5.png "Lane Lines for test5.jpg"
[image25]: ./output_images/laneline_binary_test6.png "Lane Lines for test6.jpg"
[image26]: ./output_images/lanefilled_straight_line1.png "Lane Filled for Straight_Line1.jpg"
[image27]: ./output_images/lanefilled_test1.png "Lane Filled Image for test1.jpg"
[image28]: ./output_images/lanefilled_test2.png "Lane Filled Image for test2.jpg"
[image29]: ./output_images/lanefilled_test3.png "Lane Filled Image for test3.jpg"
[image30]: ./output_images/lanefilled_test4.png "Lane Filled Image for test4.jpg"
[image31]: ./output_images/lanefilled_test5.png "Lane Filled Image for test5.jpg"
[image32]: ./output_images/lanefilled_test6.png "Lane Filled Image for test6.jpg"
[image33]: ./output_images/lanefilled_curverad_test1.png "Lane Filled with Curverad for test1.jpg"
[image34]: ./output_images/lanefilled_curverad_test2.png "Lane Filled with Curverad for test2.jpg"
[image35]: ./output_images/lanefilled_curverad_test3.png "Lane Filled with Curverad for test3.jpg"
[image36]: ./output_images/lanefilled_curverad_test4.png "Lane Filled with Curverad for test4.jpg"
[image37]: ./output_images/lanefilled_curverad_test5.png "Lane Filled with Curverad for test5.jpg"
[image38]: ./output_images/lanefilled_curverad_test6.png "Lane Filled with Curverad for test6.jpg"
[video1]: ./project_video.mp4 "Original Video"
[video2]: ./output_project_video.mp4 "Project video with Lane boundaries and curvature and position of vehicle"


## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README  -- COMPLETED

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  The write up is in Markdown format.  This write up include the explanation on how to resolve the criteria.

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./examples/example.ipynb" (or in lines # through # of the file called `some_file.py`).  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

I used the calibration1.jpg original image and show what the calibration looks like afterward.

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at lines # through # in `another_file.py`).  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `example.py` (output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[(img_size[0] / 2) - 55, img_size[1] / 2 + 100],
    [((img_size[0] / 6) - 10), img_size[1]],
    [(img_size[0] * 5 / 6) + 60, img_size[1]],
    [(img_size[0] / 2 + 55), img_size[1] / 2 + 100]])
dst = np.float32(
    [[(img_size[0] / 4), 0],
    [(img_size[0] / 4), img_size[1]],
    [(img_size[0] * 3 / 4), img_size[1]],
    [(img_size[0] * 3 / 4), 0]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
