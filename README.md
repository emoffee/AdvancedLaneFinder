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

[image1]: ./examples/image1.PNG "Undistorted"
[image2]: ./examples/image2.PNG "Road Transformed"
[image3]: ./examples/image3.PNG "Binary Example"
[image4]: ./examples/image4.PNG "Warp Example"
[image5]: ./examples/image5.PNG "Fit poly"
[image51]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/image6.PNG "Output"
[video1]: ./project_video.mp4 "Video"


###Camera Calibration

####1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

Camera varies one by one. Before any transformation of the images, we need to compute the camera matrix and distortion coefficients by chessboard approach.
I start by preparing "object points"(objp variable below), which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image. Thus, objp is just a replicated array of coordinates, and objpoints will be appended with a copy of it every time I successfully detect all chessboard corners in a test image. imgpoints will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.

![alt text][image1]

###Pipeline (single images)

####1. Provide an example of a distortion-corrected image.
I then used the output objpoints and imgpoints from camera calibration section above to compute the camera calibration and distortion coefficients using the cv2.calibrateCamera() function. I applied this distortion correction to those test images(stored in test_images folder) using the cv2.undistort() function and obtained this result after the codes below:
![alt text][image2]
####2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color thresholds & gradients to generate a binary thresholded image:
The S Channel from the HLS color space, with a min threshold of 170 and a max threshold of 255.
The SX Channel from the RGB color space, with a min threshold of 50 and a max threshold of 90 in Sobel method.

![alt text][image3]

####3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called warper(), which appears in lines 1 through 8 in the file example.py (output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook). The warper() function takes as inputs an image (img), as well as source (src) and destination (dst) points. I chose the hardcode the source and destination points in the following manner:

```
src = np.float32([[490, 480],[800, 480],
                      [1200, 720],[40, 720]])
dst = np.float32([[0, 0], [1280, 0], 
                     [1250, 720],[40, 720]])

```
This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 490, 480      | 0, 0        | 
| 800, 480      | 1280,0      |
| 1200, 720     | 1250, 720      |
| 40, 720      | 40, 720        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.
![alt text][image4]

####4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I did this with the methods learnt from class materials.

####5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this with the methods learnt from class materials.
![alt text][image5]
![alt text][image51]
####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:
![alt text][image6]

---

###Pipeline (video)

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---


###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I encountered a problem of unreliable plot onto the road such that lane area is clearly identified in the produced video. To be specific, the plotted shape keeps changing instead of staying in the form Trapezoid on the road. After several trial & error, I found I should not just use the class materials about gradient & color threshold, without even tuning min & max for different channel. 
