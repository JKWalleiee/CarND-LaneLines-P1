# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goal of this project is to make a pipeline that calculate lane lines with Opencv using Python.


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

To test the pipeline, we used a test set of images and videos provided by Udacity. In this project the following assumptions are made:
- The camera is always in the same relative position to the vehicle.
- There are no strong changes of lighting.
- There is not a high traffic of vehicles in the vicinity of the vehicle that we are controlling.

---

### Reflection

The programmed pipeline consists of 5 main stages:
- Grayscale conversion.
- Gaussian smoothing.
- Edges extraction.
- ROI segmentation.
- Lines extraction.

**Grayscale conversion**
The RGB image is converted to grayscale, this in order to have a more precise information of the changes in the image (see edges extraction). This conversion is also necessary for the OpenCV Canny detector used in the edges extraction.

**Gaussian smoothing**
This stage has two main objectives:
- Decrease the noise in the image, inherent to the camera
- Reduce the effect of the changes in the road, different to the lane lines, in the edge detection algorithm.
To meet these objectives, a Gaussian filter was used to smooth the image.

**Edges extraction**
In this stage a canny detector (link) was used to calculate the image edges. This algorithm greatly reduces number of points needed to process in the next steps.

**ROI segmentation**
In this stage, the objective is to limit the search of the lane lines to a fixed region of interest (ROI). Here, a trapezoidal ROI was designed, based on the size of the image.

**Lines extraction**
This stage is divided into two sub-stages:

- Search of raw lines:
First, the output image of the ROI stage is processed through a Hough transform algorithm, to find all the lines in the image.

- Full lines calculation:
Afterwards, the raw lines are classified in right lines or left lines depending on the value of their slope. Lines with positive slope are considered left lines, and those with negative slope are considered right lines. In addition, in this stage only lines with a slope higher than 0.5 were taken into account, this in order to eliminate any line found in the image which does not correspond to a lane line.
After having the lines divided into two sets, numpy methods are used to calculate a linear function that best fits the lines found, and a line is drawn from the bottom of the image to the horizon point of the road.


### 2. Identify potential shortcomings with your current pipeline

This pipeline has the following shortcomings:
- It may not work when the location of the camera is different to the configured for the tests
- It may not work if there are objects (cars) obstructing the view of the camera towards the road.
- If the lines on the road are very worn out, and/or there is not enough contrast between the asphalt and the line.
- If there are strong changes of lighting.

The fact of having a fixed ROI can be a problem, since the pipeline depends a lot on the position of the camera. In fact, in previous pipeline tests, there was a time when a poor ROI choice was lowering the efficiency of the line detector.

Another possible problem is that the pipeline depends a lot on the calculation of the edges, so that changes in lighting can confuse the line extractor.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use a method for the calculation of a dynamic ROI, an example would be to use an algorithm for the search of the vanishing point of the road, and use this point to calculate the ROI.
Another potential improvement could be to highlight the edges of the lines of the road, but ignoring the rest of the edges in the image. A possible solution would be to use a color detector, but using HSV images instead of RGB images, with the aim of reduce the effect of lighting in the color detection algorithm.
Another improvement would be to use the information of the lines of previous frames to calculate the current lines, from the fact that there shouldn't be much variation of the slope between sequential frames. This would help us to avoid the strong changes of slope in the lines calculated with this pipeline.

