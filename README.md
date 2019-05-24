# Finding_Lane_Lines_Advanced

## Approach

### Camera Calibration

Every time a camera captures 3D Objects in the real world, it provides a representation of it in 2D images; however, this tranformation is not exact as it modifies the sizes and shapes due to the use of curved lenses. To remove the distortion in the images a camera calibration is required, this in order to find the intrinsic parameters and then correct the images taken.

For this project, the camera calibration is performed by using chessboard images (One of the most common methods). The input data needed to calibrate the camera is a set of 3D real world points and the corresponding 2D coordinates of these points in the image.

### Undistorting images
Once the camera is calibrated, we can fix the radial and tangential distortion for each image as is shown in this section.


### Image Thresholding
After the image distortion is corrected we need to find the lane pixels. To do so, different techniques can be applied to process specific color channels and gradients to extract the pixels of our interest and create binary image that contains the lane pixels.
>

There are many different techiques that you can use to find the lane lines; however, in this project a series of color filters are applied in order to isolate the white and yellow elements in the image and therefore make the lanes clearly visible. The color filters were selected as the main method to extract the pixels of our interest, because they provided the best results in the identification of both color pixels along the images.
>

A list of the color filters and its respective colorspace used for this section is shown next:
>
* RGB White filter in the range:  (200, 200, 200) to (255, 255, 255)
* HLS Yellow filter in the range: (15, 50, 100) to (47, 220, 255)
* HSV White filter in the range: (0, 0, 205) to (255, 25, 255)
* HSV Yellow filter in the range: (21, 60, 80) to (40, 255, 255)
>

After the application of each color filter and colorspace transformations, there is a function defined to equalize the histogram of the gray image followed by a function to binarize the image. 

### Bird's Eye View
The next step of this approach is the Bird's eye transformation of the Binary Image. In this section the image is warped so we can see the a section of our interest of the original image from above. The main reason to perform this transformation is that having an image seen from above will allow the algorithm to fit a curve on the lane pixels as they are projected on a 2D plane.


### Finding the lane lines 
With the Bird's Eye view image and the lane pixels proyected on a 2D plane, the next step for the algorithm is to find the lane lines at each frame. This section of the project is divided in three parts.


1.   Histogram of the Binary Image.
2.   Apply the sliding window method.
3.   Fit a 2nd degree polynomial to the detected lane pixels.

#### Histogram
>In order to get a reference and indentify the pixels belonging to the lane lines, I apply the Histogram Method along the X axis (Columns). As the pixels in the binary image are either 0 or 1, the histogram will add the pixels in each column,  once the method performs the sum of pixels in the whole frame, two peaks corresponding to the left and right lines are found.

#### Sliding window method

>This part of the section is really important. The sliding window method starts from the centre of the peaks obtained from the histogram (in the X axis or columns), at those two points a pair of windows are created with a width of 200 pixels (100 to the left and 100 to the right) and a height of 103 pixels. 
>

>After the first pair of windows are created at the bottom of the image, six additional windows are stacked vertically in the lines shape (from the bottom to the top) and its center is determined by the mean of the pixel coordinates if the amount of pixels within that window is greater than 50.

#### 2nd Degree polynomial fit

To fit a 2nd degree polynomial that represents the left and right lines, the algorithm uses the pixel coordinates (in X and Y) obtained from the sliding window method to calculate it. 
