# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

**1) Convert image to grayscale**
I converted the images to grayscale, then I blurred the image to remove noise and spurious ingerdients by providing required kernel size. 
![image][C:\Users\anjanar2\Downloads\udacity\image.png]
![gray][C:\Users\anjanar2\Downloads\udacity\gray.png]

**2) Find canny edges**
Then I run canny edge detection algorithm that converts the grayscale image to dots(bright pixels). To capture only the lanes in the image, we find the region of interest by masking the image by an area defined by the polygon formed from vertices. The rest of the image is set to black. 
![edge][C:\Users\anjanar2\Downloads\udacity\edge.png]
![masked_image][C:\Users\anjanar2\Downloads\udacity\masked_image.png]

**3) Find hough lines**
In order to connect the dots to find any kind of shape in the image, in our case, lines, we use hough transform algorithm that takes in several parameters.
![hough_lines][C:\Users\anjanar2\Downloads\udacity\hough_lines.png]

**4) Draw the hough lines**
I, now draw the hough lines on the masked image with required colour and thickness. Then I merge this hough lines on the masked image 
to that of the original image.
![lines_origin][C:\Users\anjanar2\Downloads\udacity\lines_origin.png]

**5) Draw the full lines**
Finally I modified the draw_lines() function to form the full extent of lines from the line segments on the original image.
![full_lines][C:\Users\anjanar2\Downloads\udacity\lines.png]


In order to draw a single line on the left and right lanes, I modified the draw_lines() by :
**1) Grouping the line segments into left and right lanes**
In order to do so ,I found the slope of the hough lines because the slope determines the direction of the lines. If the slope is negative, the line moves upwards towards the horizon which means the line segments belonged to the left lane. And if the slope was positive, the line moves downwards towards the bottom, which means the line segments belonged to the right lane.

**2) Average/extrapolate the lines in each group into a single line**
For this I found the y values at the start and bottom which is common for both the lines. Now, to find the x values which are not common for both the lines, I derived polynomial equations for both the lines using polyfit and poly1D functions. This gave the linear representation of both the lines. By substituting the values of y in the equation the value of x was found. 

**3) Draw the final line**
Now, I used the x-values and the y-values to draw the full line on original image.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![image][C:\Users\anjanar2\Downloads\udacity\image.png]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the vehicle is moving along a curved road since straight lines are formed for lane detection.
Another shortcoming could be during the detection of yellow line. The lines could not be detected or formed continuoulsy due to the
difference in colour.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use some algorithm to find curved lines so that it can fit be in the assortment of dots in canny edge detected image of curved road.
Another potential improvement could be to use some ways to differentiate the colour of lane limes for smoother detection of lane lines.
