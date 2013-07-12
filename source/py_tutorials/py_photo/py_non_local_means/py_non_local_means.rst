.. _non_local_means:


Image Denoising
************************

Goal
=========

In this chapter,

    * You will learn about Non-local Means Denoising algorithm to remove noise in the image.
    * You will see different functions like **cv2.fastNlMeansDenoising()**, **cv2.fastNlMeansDenoisingColored()** etc.
    
    
Theory
=========

In earlier chapters, we have seen many image smoothening techniques like Gaussian Blurring, Median Blurring etc and they were good to some extent in removing small quantities of noise. In those techniques, we took a small neighbourhood around a pixel and did some operations like gaussian weighted average, median of the values etc to replace the central element. In short, noise removal at a pixel was local to its neighbourhood. 

There is a property of noise. Noise is generally considered to be a random variable with zero mean. Consider a noisy pixel, :math:`p = p_0 + n` where :math:`p_0` is the true value of pixel and :math:`n` is the noise in that pixel. You can a large number of same pixel (say :math:`N`) from different images and computes their average. Ideally, you should get :math:`p = p_0` since mean of noise is zero.

You can verify it yourself by a simple setup. Hold a static camera to a certain location for a couple of seconds. This will give you plenty of frames, or a lot of images of the same scene. Then write a piece of code to find the average of all the frames in the video (This should be too simple for you now ). Compare the final result and first frame. But to get a comparable result, you need :math:`N` to be very large which is not a good idea.

So idea is simple, we need a set of similar images to average out the noise. Consider a small window (say 5x5 window) in the image. Chance is large that the same patch may be somewhere else in the image. Sometimes in a small neigbourhood around it. What about using these similar patches together and find their average? For that particular window, that is fine. See an example image below:

    .. image:: images/nlm_patch.jpg
        :alt: Similar patches
        :align: center
        
The blue patches in the image looks the similar. Green patches looks similar. So we take a pixel, take small window around it, search for similar windows in the image, average all the windows and replace the pixel with the result we got. This method is Non-Local Means Denoising. It takes more time compared to blurring techniques we saw earlier, but its result is very good. More details and online demo can be found at first link in additional resources.

For color images, image is converted to CIELAB colorspace and then it separately denoise L and AB components.


Image Denoising in OpenCV
===================================

OpenCV provides four variations of this technique. 

#. **cv2.fastNlMeansDenoising()** - works with a single grayscale images
#. **cv2.fastNlMeansDenoisingColored()** - works with a color image.
#. **cv2.fastNlMeansDenoisingMulti()** - works with image sequence captured in short period of time (grayscale images)
#. **cv2.fastNlMeansDenoisingColoredMulti()** - same as above, but for color images.

Common arguments are:
    * h : parameter deciding filter strength. Higher h value removes noise better, but removes details of image also. (10 is ok)
    * hForColorComponents : same as h, but for color images only. (normally same as h)
    * templateWindowSize : should be odd. (recommended 7)
    * searchWindowSize : should be odd. (recommended 21)
    
Please visit first link in additional resources for more details on these parameters.

We will demonstrate 2 and 4 here. Rest is left for you.


1. cv2.fastNlMeansDenoisingColored()
------------------------------------------

As mentioned above it is used to noise from color images. (Noise is expected to be gaussian). See the example below:
::

    import numpy as np
    import cv2
    from matplotlib import pyplot as plt

    img = cv2.imread('die.png')

    dst = cv2.fastNlMeansDenoisingColored(img,None,10,10,7,21)

    plt.subplot(121),plt.imshow(img)
    plt.subplot(122),plt.imshow(dst)
    plt.show()
    

Below is a zoomed version of result. My input image has a gaussian noise of :math:`\sigma = 25`. See the result:

    .. image:: images/nlm_result1.jpg
        :alt: Result of denoising
        :align: center
        
        
        
        
Additional Resources
========================

#. http://www.ipol.im/pub/art/2011/bcm_nlm/ (It has the details, online demo etc. Highly recommended to visit. Our test image is generated from this link)

#. Online course at coursera (link to be added)(First image taken from here)

Exercises
============        
