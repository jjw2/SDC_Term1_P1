# **Finding Lane Lines on the Road** 
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the work in a written report


[//]: # (Image References)

[image1]: md_imgs/orig.png "Original"
[image2]: md_imgs/color_slct.png "Color Selection"
[image3]: md_imgs/gray.png "Grayscale"
[image4]: md_imgs/blur.png "Blurring"
[image5]: md_imgs/canny.png "Cannt Edge Detection"
[image6]: md_imgs/roi.png "Region of Interest Selection"
[image7]: md_imgs/final.png "Final"

---

###  Pipeline Description ##
Answer to Question 1 from Udacity [rubric XXX](link).

The following sections describe the steps in the image processing pipeline. These steps are illustrated as applied to the image below:

![alt text][image1]

**1) Color Selection**
As a first step, I isolated only the colors in the image with which the systems is concerned - yellow and white (i.e.: the colors of the lane lines) - leaving all other pixels black/empty. My assumption here was that this would filter out a significant number of lines in the image that would otherwise have been created later in the pipeline during canny edge detection.

For color selection, images were converted to the HSV color space, which made selection of specific colors easier (see this link for a description: [XXX LINK HERE](link)).

In some cases, parts of the sky ended up being included in the selected colors (i.e.: they were white). These portions end up being filtered out later in the pipeline during Canny edge detection and region of interest selection.

![alt text][image2]

**2) Grayscale Conversion**
Images were converted to grayscale, as required by the Canny edge detection algorithm.

![alt text][image3]

**3) Gaussian Blurring**
A Gaussian blur was applied, as suggested in the Udcaity course work.

![alt text][image4]

**4) Canny Edge Detection**
Canny edge detection was performed on the images. Paramters were tuned manually until appropriate results were achieved.

![alt text][image5]

**5) Region of Interest Selection**
A general mathematical formula was applied to select only a portion of the image.

![alt text][image6]

**6) Hough XXX - WHAT IS THIS CALLED?**
A Hough XXX was applied to capture lines in the image. Paramters were tuned manually until appropriate results were achieved.

**7) Slope Selection and Averaging (w/in Draw Lines Function)**
The results of the Hough XXX were filtered, to extract only lines within a range of slopes. This was performed separately for left and right lane lines (i.e.: they have different slopes). It's important to note that the y-intercept of each line was not considered, since there is no guarantee that it will remain the same over time. Generally, the same can be said of filtering just on the basis of slope, so care must be taken not to be too selective.

Once lines were filtered for slopes, each set of left and right lane lines - both slope and y-intercept - were averaged together, and the final mean slope and y-intercepts were used to generate final left and right lane lines, which were plotted on the image. 

![alt text][image7]


### 2. Potential Shortcomings and Possible Improvements
Answer to Questions 2 and 3 of Udacity [rubric XXX](link).

The following outline potential shortcomings and possible improvements to the pipeline:

**1) Dataset Size**
This isn't necessarily a shortcoming of the pipeline itself, but it's been validated only on a very small dataset. The dataset is also very homogenous, containing a small subset of the all possible driving scenarios (ex: daytime, nighttime, good/bad weather, various road conditions, various line qualities (ex: new versus faded lines), and the list goes on...). Even given the small sample size, the pipeline struggled in some areas, such as when the shadow of a tree passed over lane lines, or when the road switched from asphalt to concrete and lines were harder to make out.

**2) Use of Historical/Temporal Information**
Currenly, the pipeline examines each image individually, without considering the position of the lane lines in time. As a result, the lines "jitter" throughout the videos as they change freely in each frame.  

Lines on the road change slowly enough over time (depending on sample time) that considering historical information - by using a weighted average of the lane line definitions over time, for example - would be beneficial. This may reduce the lane "jitter" seen in the video. 

**3) Lane Line Filtering**
The algorithm currently averages all perceived lane lines (see **Slope Selection and Averaging** above), meaning that all lines, regardless of size, etc, are applied equally to average. The algorithm could be improved by applying a more complex algorithm to appropriately weight lines that might more clearly depict a line direction (ex: weighted average based on line length). 

**4) Consideration of Curvature**
In general, the lane lines in the dataset are relatively straight, with only low rates of curvature. Higher rates of curvature may cause issues with "line" detection. 

The algorithm could be improved by using an elliptical fitting technique such as that shown [here XXX](link). Road/lane curvature could also be beneficial for use as an input to path planning/navigation algorithms as well (in the absence of more robust information such as from mapping data). This has the potential to be rather computationally intensive.

**5) Handling Traffic**
While not shown in the test videos, traffic may obstruct lines (ex: vehicles performing lane changes), and the current algorithm will struggle with this. Historical information (as discussed in point 2 above) could help with this. 
