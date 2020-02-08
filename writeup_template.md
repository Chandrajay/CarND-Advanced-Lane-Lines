## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

In this project I used computer vision tools and techniques to detect the lanes, curvature and the offset centre from the lane

The steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)



[image1]: ./output_images/undistort_result_chessboard.jpg "Undistorted"
[image2]: ./output_images/undistort_result_Img.jpg "Undistorted"
[image3]: ./output_images/Combin_s_Grad_thresh.jpg "Binary Example"
[image4]: ./output_images/Birdseye.jpg "Birds Eye"
[image7]: ./output_images/BirdsEye_Combin_binary_img.jpg "Birds Eye Binary"
[image5]: ./output_images/Lane_Pixels.jpg "Lane Pixels"
[image6]: ./output_images/Final_image.jpg "Final Image"
[video1]: ./project_video_output.mp4 "Output video"
[video2]: ./challenge_video_output.mp4 "Output video challenge"
[video3]: ./Harder_challenge_video_output.mp4 "Output video Harder"



### Camera Calibration


The code for this step is contained in the second code cell of the IPython notebook located in "proj2.ipynb" 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2.Binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at lines 2 through 38 in the 13th code cell.  Here's an example of my output for this step. 

![alt text][image3]

#### 3.Perspective transform 

The code for my perspective transform includes a function called `warp() and warp1()`, which appears in 14th cell code and 17th cell code in the file `proj2.ipynb`.  The `warp()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```
 src = np.float32(
        [[200, height],
         [570,470],
         [760,470],
         [1160, height]])
    
    dst = np.float32(
       [[210, height],
        [210, 0], 
        [1100, 0],  
        [1100, height]])
    
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 200, 720      | 210, 720      | 
| 570, 470      | 210, 0        |
| 760, 470      | 1100, 0       |
| 1160, 720     | 1100, 720     |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]
![alt text][image7]

#### 4. Identification of Lane Lines

Then I used a function 'find_lane_pixels()' where I took a histogram of the image which gives the peak points that signifies that there is a lane present, Next I applied a window search to find the peaks for the left and right lane points. Then I create a window that searches from the bottom of the image to Identify the nonzero pixels in x and y within the window. 

![alt text][image5]

#### 5. Radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in 24th cell code, using the formula given in the lecture and shaded the region between lane lines as shown below:

![alt text][image6]

---

### Pipeline (video)


Here's a [link to my project video result](./project_video_output.mp4)

Link for [challenge video](./challenge_video_output.mp4)
 
 Link for [Harder challenge video](./Harder_challenge_video_output.mp4)


---

### Discussion

Challenging Part

The most challenging part of this model is to find the lanes when the road conditions (Color of the road has a brighter gradient and white shades), A proper threshold filter can be used to detect the exact lane lines in the video.

The places where the lanes are not visible and have a high light focus on the camera the camera goes blind and will not be able to detect the lanes if the road is cureved during the above mentioned scenarios, We cannot use the previous lane lines and proceed to predict the lanes
