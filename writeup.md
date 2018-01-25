# **Finding Lane Lines on the Road** 
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[grayscale]: ./imgs/grayscaled.jpg "Grayscale"
[canny_applied]: ./imgs/canny_applied.jpg "Canny-applied"
[blurred]: ./imgs/blurred.jpg "Blurred"
[masked]: ./imgs/maksed.jpg "Masked"
[hough]: ./imgs/hough.jpg "Hough"
[final]: ./imgs/final.jpg "Final"
[final2]: ./imgs/final2.jpg "Final2"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of the following steps.
* Convert the given image to grayscale.
![alt text][grayscale]
* Apply the Canny transform to the grayscaled image to detect edge. (low_threshold=50, high_threshold=150)
![alt text][canny_applied]
* Apply a Guassian Noise kernel. (kernel_size=5)
![alt text][blurred]
* Filter out noises by masking center area (regions of interest=(0.06W, H), (0.44W, 0.62H), (0.56W, 0.62H), (0.94W, H))
![alt text][masked]
* Calculate hough line (rho=2, theta=1 degree, threshold=40, min_line_len=60, max_line_gap=50)
![alt text][hough]
* Extrapolate lane lines from hough line segments.
![alt text][final]
* Final result
![alt text][final2]

In order to extrapolate two single lines from hough line segments, I first categorized line segments by x values into two parts: left and right. If two ends of a line segments is left side of image, we can consider it as a part of a left lane. The same approach can be applied to the right lane. After then, I filter out segments which has unaccountable slopes. For example, if a segment has 0 degree, which means horizontal line, we can regard it as a noise because lane could not be a horizontal lane. I calculated average values of x values, y values, and slopes from filtered line segments, each for left and right category. And I drew two line from this value.

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when driving extremely curved lanes. Because we extrapolated lanes with the assumption that the lane will be straight(ish), if the lane is extremely curved such as road on mountain, this algorithm could not be able to detect lane properly.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to take color information into consideration. There's only limited number of colors which can be used on lane: yellow, white, and blue. If we can filter out other colors, our algorithm will have better accuracy.
