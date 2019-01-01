# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve_annotated.jpg "solidWhiteCurve_processed"
[image2]: ./test_images_output/solidYellowCurve_annotated.jpg "solidYellowCurve_processed"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Here is my step by step approach and my thought process on Building Pipeline:
- Images have lanes with (White and Yellow) color. I have tried to see images in different Color spaces RGB,HSV,HLS.
- Converted RGB image to HSV and HSL space .Using Reference: [RGB Color Code Chart](http://www.rapidtables.com/web/color/RGB_Color.htm)
- In my observation HSV yellow lines are clear but not white lines in shades , HSL both Yellow and white lines are clear.
- Select white and yellow color , combine using bitwise_or and map it to image using bitwise_and function.
- Convert masked white and yellow color to Grayscale image using Grayscale function.
- Next step is to reduce noise by using GaussianBlur which take kernel size as one of parameter and it takes odd values (1,3,5,7 etc)
  larger Kernel size more blur image would become
- Apply Edge Detection using Canny algorithm.Canny take low_threshold and high_threshold as parameter by documention it should be in ratio of 1:2 or 1:3
-  Once you get Edges from Canny now we need to select particular region or set of edges. We obtaine it by using ROI Which take vertices as prameter 
- Hough Transform line detection algorithm is used to detect line in edge images. It has several parameters to tune 
    - rho – Distance resolution of the accumulator in pixels.
    - theta – Angle resolution of the accumulator in radians.
    - threshold – Accumulator threshold parameter. Only those lines are returned that get enough votes (> `threshold`).
    - minLineLength – Minimum line length. Line segments shorter than that are rejected.
    - maxLineGap – Maximum allowed gap between points on the same line to link them.
- Output of Hough Transform line algorithm is list of lines and next step is to Draw line on image
- I observe that there are multiple lines detected for Lane, We need it be single averaged line .Also for some lane are partially recognized.
- For Averaging and extrapolating Lines - Separate positive slope line (Right lane) and negative slope line (Left line) and average them.
using average_slope_intercept fuction
- Using make_point function convert slope and intercept into pixel points .
- Finally Draw_lane_line function which take those list of lines which in form of pixel points and draw averaged and extrapolated lanes on image .


![alt text][image1]
![alt text][image2]

- Same pipeline is applied on video clips and images out put stored in outputfiles folder for reference and my p1.ipynb file has clear info for reference.

### 2. Identify potential shortcomings with your current pipeline

I probably doubt that my pipeline work in different light conditions like night or bad whether and curve roads.




### 3. Suggest possible improvements to your pipeline

I need to test with some other road lane images in different conditions and improve algorithm based on results it show.
