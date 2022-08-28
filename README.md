# OpenRoad
## Project Description
This is an attempt to make a smart driving assistant to help people drive better on road and prevent road accidents.

## Things it involved
* ### Software
    * Google Colab
    * YOLO and OpenCV
    * Keras and Pytorch
* ### Hardware
    * Proximity sensor and Ultrasonic sensor
    * Single Board Computer (Raspberry Pi)
    * Camera

## 1. Lane Detection
Lane Detection uses Computer Vision techniques to develop a robust algorithm that can detect and track lane boundaries in a video or camera-feed.
* It can detect exactly two lane lines; the left and right lane boundaries of the lane the vehicle is currently driving in.
* It cannot detect adjacent lane lines.
* The vehicle must be within a lane and must be aligned along the direction of the lane.
* Output is generated based on the moving average of the previous detections if lanes are not clearly visible in current frame.

<img width="360" alt="image1_1" src="https://user-images.githubusercontent.com/23453334/187064291-37fead03-f6fb-441a-93f6-5f6f3c5a9d41.png">
<img width="360" alt="image1_2" src="https://user-images.githubusercontent.com/23453334/187064316-3ac2b363-20f4-4387-916c-5bdc6d55b135.png">


## Perspective Transformation and Region Of Interest (ROI) selection

Perspective Transformation which warpes the image into a bird's eye view scene; as if the camera was looking vertically downward from above the road (and not mounted on a vehicle moving horizonatlly with respect to the road). This makes it easier to detect the lane lines and measure their curvature.

1. Compute the transformation matrix by passing the source and destination points into cv2.getPerspectiveTransform.
2. Then, the undistorted image is warped by passing it into cv2.warpPerspective along with the transformation matrix.
3. Finally, we cut/crop out the sides of the image using a utility function get_roi() since that portion of the image contains no relevant information.


## Generating a thresholded binary image
The surface on both sides of the lane lines has different brightness and/or saturation and a different hue than the line itself, and, Lane lines are not necessarily contiguous, so the algorithm needs to be able to identify individual line segments as belonging to the same lane line.

The masking process needs to be useful enough to account for uneven road surfaces and most importantly non-uniform lighting conditions.

RGB, HLS, HSV masks were used. Each of these masks were composed through a Logical OR of two sub-masks created to detect the two lane line colors of yellow and white. Moreover, the threshold values associated with each sub-mask was adaptive to the mean of image / search window (further details on the search window has been provided in the sub-sections below)

Logically, this can explained as:
`Final Mask = Sub-mask (white) | Sub-mask (yellow)`


<img width="360" alt="image2_ (2)" src="https://user-images.githubusercontent.com/23453334/187064352-a56d982e-fa19-4b84-98cb-3a81f5c2a32b.png">


[Google Colab Notebook](https://colab.research.google.com/drive/1ed85zVZwgemhxHf-DPHbek4qkkj-TJCP?usp=sharing)

<hr>

## 2. Vehicle Detection and Tracking
Detecting objects in a video using YOLO V3 algorithm. The approach is quite similar to detecting images with YOLO. We get every frame of a video like an image and detect objects at that frame using yolo. Then draw the boxes, labels and iterate through all the frame in a given video.
OpenCV was used to capture video frames.


https://user-images.githubusercontent.com/23453334/187064391-19c1fb3b-3dbe-4d2c-9f41-78c518d2782a.mp4

[Google Colab Noteboot](https://colab.research.google.com/drive/1qWp8vseGVr_9dRH1FV_oPXlr2Bn7LbhH?usp=sharing)

<hr>
