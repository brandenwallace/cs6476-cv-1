## Problem Set 0 – Images as Functions (really arrays or matrices of numbers)

This problem set is really just to make sure you can load an image, manipulate the values, produce some output.

### 1. Input images
Find two interesting images to use. They should be color. You might find some classic vision examples in [http://sipi.usc.edu/database/database.php?volume=misc](http://sipi.usc.edu/database/database.php?volume=misc). Or take your own. Make sure they are not larger than 512x512. 
Output: both images

### 2. Color planes
1. Swap the red and blue pixels of image 1  
Output: new image  
2. Create a monochrome image (M1g) by selecting the green channel of image 1  
Output: new image  
3. Create a monochrome image (M1r) by selecting the red channel of image 1  
Output: new image  
4. Which looks more like what you’d expect a monochrome image to look like? Would you expect a computer vision algorithm to work on one better than the other?  
Output: response  

### 3. Replacement of pixels
Take the inner square of 100x100 pixels of monochrome version of image 1 and insert them into the monochrome version of image 2  
Output: new image  

### 4. Arithmetic and Geometric operation
1. What is the min and max of the pixel values of M1g? What is the mean? What is the standard deviation? And how did you compute these?  
Output: response  
2. Subtract the mean from all the pixels, then divide by the standard deviation, then multiply by 10 (if your image is zero to 255) or by 0.05 (if your image ranges from 0.0 to 1.0). Now add the mean back in.  
Ouptut: new image  
3. Shift M1g to the left by 2 pixels.  
Output: new image  
4. Subtract the shifted version of M1g from the original and make sure that the values are legal (what do negative numbers for pixels mean anyway?).  

### 5. Noise
1. Take the original colored image and start adding Gaussian noise to the pixels in the green channel. Increase sigma until the noise is somewhat visible.  
Output: new image. Response: what is sigma?  
2. Now, instead add that amount of noise to the blue channel.  
Output: new image  
3. Which looks better? Why?  
Output: response  
