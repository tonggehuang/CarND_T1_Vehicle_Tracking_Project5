
### Vehicle Detection and Tracking

---

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/data_preview.png
[image2]: ./output_images/combination.png
[image3]: ./output_images/features_vis.png
[image4]: ./output_images/labeled_heatmap.png
  
---
### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`). As suggested by lectures, I will used: 

* All pixel intensities of resized images
* Hist features for all L, U and V channel
* HOG feature of all three color channels. 

I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like, and the histgram features look like. The `LUV` channel used in my pipeline is because the difference between the hist features for two classes can be visualized. 

![alt text][image3]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters. Here is the grid of combinations results.

![alt text][image2]

The vehicle detection and tracking should be used for real time applications, besides the test accuracy, the prediction speed should also be considered. Also, the feature extraction time and vector length should also be token into considerations to reduce computation load. So, the combination 1 will be used in my pipeline. Then, I trained a linear SVM using the combination features metioned above. 

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I decided to used four different sliding windows based on scale and ROI. I used the small scales and windows for searching the upper part of the image, since the vehicle seems "small" on the far points. For the lower part of the image, the car looks "bigger", so I used larger sliding windows.

The hot windows and threshold also used to eliminate false positives on images. 

![alt text][image4]

---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](https://youtu.be/-_3eJISTUt8)


---

### Discussion

1. The whole process and quality output is highly depend on the parameter and sliding window setting. For example, I limited and modified the searching area by observing detection results. In other words, if the road condition is very different from the project video, my pipeline might fail to detect and track the vehicles. So the more intelligent and robust searching area determination method shoule be used in the future. 


2. For the purpose of real time application, my pipeline took around 5 minustes to process the 1 minutes video. So, my pipeline can't be used for real time applications. The more effecient method should be used for detection and tracking.

