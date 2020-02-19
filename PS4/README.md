## Problem Set 4: Harris, SIFT, RANSAC

The focus of this problem set is on feature computation and model fitting. As defined in class features must be (1) fairly repeatable - will tend to show up in both images even with changes in lighting or imaging; (2) well localizable - their location in the imagery should be easily and relatively precisely determined; (3) fairly common without being dense in the imagery; (4) characterizable such that it is possible to find likely matches. Once we have such features and their putative matches, we use RANSAC as a form of global checking to find a likely alignment. You will do each of these steps below, though for some of them code will be provided.

The images _transA.jpg_ and _transB.jpg_ are a pair of images that are related by a pure translation. Likewise _simA.jpg_ and _simB.jpg_ are images related by a similarity transformation (more on this later). You will use both of these sets for evaluation and testing of your code. Additionally we provide you with two simple images of a checkerboard _check.bmp_ and _checkrot.bmp_ which may be easier to use in testing your corner detection code. 

### 1. Harris corners
In class and in the text we have developed the Harris operator. To find the Harris points you need to compute the gradients in both the X and Y directions. These will probably have to be lightly filtered using a Gaussian to be well behaved. You can do this either the “naive” way - filter the image and then do simple difference between left and right (X gradient) or up and down (Y gradient) - or you can take an analytic derivative of a Gaussian in X or Y and use that filter. The scale of the filtering is up to you. You may play with the size of the Gaussian as it will interact window size of the corner detection. 
1. Write functions to compute both the X and Y gradients. Try your code on both transA and simA. To display the output, adjoin the two gradient images (X and Y) to make a new, twice as wide, single image (the ”gradient-pair”). Since gradients have negative and positive values, you’ll need to produce and image that is gray for 0.0 and balck is negative and white is positive.  
Output: The code, and the gradient-pair image for both transA and simA.

Now you can compute the Harris value for the image. As a reminder the Harris value was define as:  
![R = \det M - \alpha\trace(M)^2](https://render.githubusercontent.com/render/math?math=R%20%3D%20%5Cdet%20M%20-%20%5Calpha%5Ctrace(M)%5E2)  
where  
![M = \sum_{x,y} w(x,y) \begin{bmatrix} I_x^2 & I_x I_y\\ I_x I_y & I_y^2 \end{bmatrix}](https://render.githubusercontent.com/render/math?math=M%20%3D%20%5Csum_%7Bx%2Cy%7D%20w(x%2Cy)%20%5Cbegin%7Bbmatrix%7D%20I_x%5E2%20%26%20I_x%20I_y%5C%5C%20I_x%20I_y%20%26%20I_y%5E2%20%5Cend%7Bbmatrix%7D)  
The only design decisions are the size of the window (the sum), the windowing function that controls the weights, and the value of α in the Harris scoring function.

2. Write code to compute the Harris value. You can try the weights just equal to 1. But it might work better with a smoother Gaussian that is higher at the middle and falls off gradually. Your output is a scalar function. Apply to transA, transB, simA, and simB.  
Output: The code and the Harris value output image for each of the images. To display the output reasonably you will have to scale the image values to be in a range of 0-255 or 0.0 to 1.0, depending upon how you deal with images.

Finally you can find some corner points. To do this requires two steps: thresholding and non-maximal suppression. You’ll need to choose a threshold value that eliminates points that don’t seem to be plausible corners. And for the non-maximal suppression, you’ll need to choose a radius (could be size of a side of a square window instead of a circle radius) over which a pixel has to be a maximum.

3. Write a function to threshold and do non-maximal suppression on the Harris output. Surprise, huh? Adjust the threshold and radius until you get a “nice” set of points, probably on the order of a hundred or two (or three?). But use your judgment in terms of getting enough points. Are there any points that are not found in both images?  
Output: The code. Apply your function to both image pair: (transA, transB) and (simA, simB). Mark the corners visibly in each of the four result images and provide those images. Also, describe the behavior of your corner detector including anything surprising, such as points not found in both images of a pair. 

### 2. SIFT features
Now that you have keypoints for both image pairs, we can compute descriptors. You will be glad to know that we do not expect you to write your own SIFT descriptor code. Instead you’ll use either a MATLAB package called VLFeat or the SIFT or SURF classes in OpenCV. See the accompanying supplemental document for details.

The standard use of a SIFT library consists of you just providing an image and the library does its thing: finds interest points at various scales and computes descriptors at each point. We’re going to use the code just to compute the orientation histogram descriptors for the interest points you have already detected from Problem 1. To do so, you need to provide a scale setting and an orientation for each feature point as well as the gradient magnitude and angle for each pixel. The scale we’ll fix to 1.0 (see accompanying SIFT software usage tutorial). The orientation needs to be computed from the gradients:  

![angle = \atan2(I_y,I_x)](https://render.githubusercontent.com/render/math?math=angle%20%3D%20%5Catan2(I_y%2CI_x))

But, you already have the gradient images! So you can create an “angle” image and then for a given feature point at <ui,vi> you can get the gradient direction.

1. Write the function to compute the angle. Then for the set of interest points you found above, plot the points for all of transA, transB, simA and simB on the respective images and draw a little line that shows the direction of the gradient. In MATLAB, if you want you can use the VLFeat function vl_plotframe to draw the feature points locations and the angle. You’ll need to figure it out - look at http://www.vlfeat.org/overview/sift.html and also the documentation for vl_plotframe. In OpenCV you can use the method drawKeypoints().  
Output: The code. And both of the drawn on pairs (transA,transB) and (simA,simB).

Now we’re going to call the SIFT descriptor code. You need to pass in each keypoint’s location along with its scale and orientation. This process is covered in more detail in the accompanying supplemental document.  

Once we have the descriptors, we need to match them. This is what we called _putative matches_ in class. Given keypoints in two images, get the best matches. Both VLFeat and OpenCV have functions for computing matches. You will call those and then you will make an image that has both the A and B version adjoined and that draws lines from each keypoint in the left to the matched keypoint in the right. We’ll call this new image the putative-pair-image. Now, both of the SIFT packages have functions to do this. **But you are not permitted to use them!!!** This way you will have to identify the location of each keypoint and its match and explicitly draw the line. This tells us you have a good handle on the data structures that you are using.  

2. Write the function to call the appropriate SIFT descriptor extraction function with the necessary input data structures. Do this for all the keypoints in both pairs of images. Then call the matching functions of VLFeat or OpenCV to compute the best matches between the left and right images of each pair. Then create the putative-pair-image for both transA-transB and simA-simB pair. You must write your own drawing function (note you may use OpenCV’s line() function or MATLAB’s plot() function).  
Output: The code. And both of the putative-pair-images.  

### 3. RANSAC
We’re almost there. You now have keypoints, descriptors and their putative matches. What remains is RANSAC. To do this for the translation case is easy. Using the matched keypoints for transA and transB, randomly select one of the putative matches. This will give you an offset (a translation in X and Y) between the two images. Find out how many other putative matches agree with this offset (remember, you may have to account for noise, so ”agreeing” means within some tolerance). This is the consensus set for the selected first match. Find the best such translation - the one with the biggest consensus set.

1. Write the code to do the translational case ontransAandtransB. Draw the lines on the adjoined images of the biggest consensus set.  
Output: The code and the drawn upon adjoined image pairs. Also, say what the translation vector was and what percentages of your matches was the biggest consensus set.  

For the other image pair we need to compute a similarity transform. Recall that a similarity transform allows translation, rotation and scaling. We can represent this transform with a matrix as:  
![\begin{bmatrix}u'\\v'\end{bmatrix}](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bbmatrix%7Du'%5C%5Cv'%5Cend%7Bbmatrix%7D)
![= \begin{bmatrix} a & -b & c \\ b & a & d \end{bmatrix}](https://render.githubusercontent.com/render/math?math=%3D%20%5Cbegin%7Bbmatrix%7D%20a%20%26%20-b%20%26%20c%20%5C%5C%20b%20%26%20a%20%26%20d%20%5Cend%7Bbmatrix%7D)
![\begin{bmatrix}u\\v\\1\end{bmatrix}](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bbmatrix%7Du%5C%5Cv%5C%5C1%5Cend%7Bbmatrix%7D)

In other words, there are four unknowns. Each match gives two equations - so you need to pick two matches to solve, which is why similarity is called a two point transform. Clever.

2. Do the same as above but for the similarity pair simA and simB. Write code to apply RANSAC by randomly picking two matches, solving for the transform, and determining the consensus set. Draw the lines on the adjoined images for the biggest consensus set.  
Output: The code and the drawn upon adjoined image pairs. Also, say what the transform matrix is for best set and what percentages of your matches was the biggest consensus set.
