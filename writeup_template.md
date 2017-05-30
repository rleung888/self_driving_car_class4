## Writeup Template

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

By: Raymond Leung
Date: 2017-05-29

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

The code for this step is contained in the first code cell of the IPython notebook located in "./examples/example.ipynb" 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

I used the calibration1.jpg original image and show what the calibration looks like afterward.

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

Get one of the straight line image and apply cv2.undistort function. Use thr camera matrix mtx and the disCoeffs dist obtain the calibration.  The output image is the undistorted image

![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I chose the S-Channel and SobelX, SobelY, Magnitude and Direction combined threshold.   I also did an experiment with S-Channek and SobelX only.  There aren't much different from the image I get.  These are the steps I get to the final combined binary:

SobelX Gradient Threshold
![alt text][image3]
SobelY Gradient Threshold
![alt text][image4]
Magniture Gradient Threshold
![alt text][image5]
Direction Gradient Threshold
![alt text][image6]
Combined the SobelX, SobelY, Magniture and Direction Gradient
I used Kennel Size = 3 for each gradient, the threshold on earch time pretty much keep the same as in the lesson.
![alt text][image7]
S Channel output
![alt text][image8]
Combined S Channel and SobelX gradient
![alt text][image9]
Combined S Channel and Combined gradient.  
![alt text][image10]

It looks like the S Channel and Combined gradient binary has fewer objects on the side of the road than S Channel and SobelX binary.  So for the rest of the calculation.  I use this format.


#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `example.py` (/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  

I started with the hard coded position from the straight_line1.jpg.  

```python
    src = np.float32([[599,450],[258,685],[1051,685],[681,450]])
    dst = np.float32([[258,0], [258, 720], [1051,720],[1051,0]])
```    

However, I try to avoid the problem on different image size, I change the hard coded position into percentage


    [599,450] = width x 0.4680, height x 0.625     
    [258, 685] = width x 0.2016, height x 0.9514    
    [1051, 685] = width x 0.8211, height x 0.9514    
    [681, 450] = width x 0.5320, height x 0.625

I create a offset to bring the X-axis closer to keep the line within the image.   Some of the wide turn have the line out of the screen.  With the line closer, it will not affect the curvature.
```python
    offset = 150
    src = np.float32([[w*0.4680, h*0.6250],[w*0.2016,h*0.9514],[w*0.8211,h*0.9514],[w*0.5320,h*0.625]])
    dst = np.float32([[w*0.2016 + offset,0], [w*0.2016 + offset, img.shape[0]], [w*0.8211 - offset,img.shape[0]],[w*0.8211 - offset,0]])
```

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image11]

Applied to other test images.
![image12]
![image13]
![image14]
![image15]
![image16]
![image17]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?


First of all, apply the histogram on the image to identify the peak area of image.  The left peak is the left land and right peak is the right lane. 
```python
histogram = np.sum(binary_warped[binary_warped.shape[0]//2:,:], axis=0)
```
The second part is the using the sliding windows method to draw on the visualization image
[image18]
Then append the indics to the list
Applied the second ploynomail to the array

```python
    left_fit = np.polyfit(lefty, leftx, 2)
    right_fit = np.polyfit(righty, rightx, 2)
```

![alt text][image18][image19]

And also applied the same for the test images
![alt text][image20]
![image21]
![image22]
![image23]
![image24]
![image25]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

For curvature, I use the parameter from the example for my calculation.  I used the sameple code of the lesson to build the curvature.
    ym_per_pix = 30/720 # meters per pixel in y dimension
    xm_per_pix = 3.7/700 # meters per pixel in x dimensio
I applied the ploynomial and fit the value into the formala to get teh left curvature in meter and right curveture in meter
    
I found out if the value is super large, more likely > 5000, it represent it is a straight line.   

For the position of the vehicle, I have the average of the left lane and right lane bottom.  Then I have it minus the center of the image which I assume the center of the camera.   Multiple it by xm_per_pix is the position.



#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I applied the pipeline on the test images

![alt text][image33]
![image34]
![image35]
![image36]
![image37]
![image38]


---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

![video2]
Here's a [link to my video result](./output_project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I still see some shadow on my warped images.   I tried the pipeline on the challenge video.   It basically failed because the car is on the right side of a curved road, it is including picture on the incoming traffic on the opposite side.   The left lane and right lane are both on the area where >= image.shape[1]/2.   So it will failed to detect the left lane and use the further lane as the left lane.  

