## Problem Set 2 – Window based Stereo Matching

In class and in Forsyth and Ponce, chapter 7 we discussed window-based approaches to estimating dense stereo correspondence. In this project you will implement such approaches and evaluate it on some standard stereo pairs.

1. For this first problem we provided a test pair *leftTest.png* and *rightTest.png* that are just a central square moved 2 pixels to the right.  
![](../../media/PS2/1a.png)  
Implement the basic stereo algorithm of taking a window around every pixel in one image and search for the best match along the same scan line in the other image. **You will do this both left to right and right to left.** Remember: because of depth changes (discontinuities) some pixels visible in the left image are not in the right and vice a versa. So you will match in both directions.  
For this part implement the simplest thing imaginable: look for the smallest difference between the template window (source) and the proposed location window. Use the sum of squared differences measure (SSD).  
SSD is defined by:  
![](../../media/PS2/1b.png)  
Basically you just sum up the squares. A “good” match, then, is when this value is at a minimum. That is, you are looking for the same image patch in both images. **YOU MAY NOT USE BUILT-IN SSD FUNCTIONS THAT BASICALLY DO THIS ENTIRE STEP.** But it is easy in Matlab to sum the squares of a matrix; check out the operator: .^2 (Yes you need the ‘dot’.)
    1. Implement the SSD match algorithm, and create a disparity image D(x,y) such that L(x,y) = R(x+D(x,y),y) when matching from left to right. Also match from right to left.  
    Output: Show DL(x,y) [matching from left to right] and DR(x,y)[matching from right to left] as images. These disparity images may need to be scaled and shifted to display correctly. They should show a central square moved 2 pixels to the left or right.
    
2. Now we’re going to try this on a real image pair: *proj2-pair1-L.png* and *proj2-pair1-R.png*. Since these are color images create gray scale versions. You can use **rgb2gray** or your own function.  
    1. Again apply your SSD match algorithm, and again create a disparity image D(x,y) such that L(x,y) = R(x+DL(x,y),y) when matching from left to right. Also match from right to left.  
    Output: Show DL(x,y) [matching from left to right] and DR(x,y)[matching from right to left] as images. These disparity images may need to be scaled and shifted to display correctly.

    2. Also in this directory are the ground truth disparity images *proj2-pair1-Disp-L.png* and *proj2-pair1-Disp-R.png*. Compare your results.
    Output: description of the differences between your results and the ground truth

3. SSD is not very robust to certain perturbations. We’re going to try to see the effect of perturbations:  
    1. Add some Gaussian noise to the image; either one or both. Make the noise sigma big enough that you can tell some noise has been added. Run SSD again.  
    Output: Disparity images and analysis of result compared to part 1.

    2. Instead of the Gaussian noise, increase the contrast (multiplication) of one of the images by just 10%. Run SSD again.
    Output: Disparity images and analysis of result compared to part 1.

4. Now you’re going to **use** (not implement yourself unless you want) an improved method, called **_normalized correlation_** – this is discussed in the book. The basic idea is that we think of two image patches as **_vectors_** and to compute the angle between them – much like normalized dot products. This can be written as:
![](../../media/PS2/4a.png)  
where the above is just the explicit dot product which then has to be normalized by the magnitude of each:
![](../../media/PS2/4b.png)  
    1. Implement a window matching stereo algorithm using some form of normalized correlation. **Matlab has its own function normxcorr2(template, A)** which implements:  
    ![](../../media/PS2/4c.png)  
    OpenCV has a variety of relevant functions as well such as CV_TM_CCOEFF_NORMED. **You MAY use these built in normalized correlation functions.**  
    Test it on the original images both left to right and right to left.  
    Output: disparity images and description of how it compares to the SSD version and to the ground truth
    2. Now test it on both the Gaussian noise and contrast boosted versions from 2a and 2b.  
    Output: Again disparity images and analysis of results.

5. Finally, there is a second pair of images: _proj2-pair2-L.png_ and _proj2-pair2-R.png_  
Try your algorithms on this pair. Play with the images – smooth, sharpen, etc. Keep comparing to the ground truth.  
Output: disparity images and analysis of what it takes to make stereo work using a window based approach.
