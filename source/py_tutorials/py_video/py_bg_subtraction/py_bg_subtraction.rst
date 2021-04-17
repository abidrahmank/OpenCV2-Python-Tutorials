.. _background_subtraction:


Background Subtraction
****************************

Goal
=======

In this chapter,

    * We will familiarize ourselves with the background subtraction methods available in OpenCV.
    
Basics
=============

Background subtraction is a major preprocessing step in many vision based applications. For example, consider cases like a visitor counter where a static camera takes the number of visitors entering or leaving the room, or a traffic camera extracting information about vehicles etc. In all of these cases, you need to first extract the person or vehicles. Technically, you need to extract the moving foreground from static background.

If you have an image of the background alone, like an image of the room without visitors, or of the road without vehicles, it is an easy job. Just subtract the new image from the background and you will get the foreground objects alone. But in most of cases, you may not have such an image, so we need to extract the background from whatever images we have. It becomes more complicated when the objects have shadows (i.e. shadow of vehicles). Since the shadow is also moving, simple subtraction will mark that also as foreground, further complicating things.

Several algorithms were introduced for this purpose. OpenCV has implemented three such algorithms which are very easy to use. We will see them one-by-one.


BackgroundSubtractorMOG
-----------------------------------

BackgroundSubtractorMOG is a Gaussian Mixture-based Background/Foreground Segmentation Algorithm. It was introduced in the paper "An improved adaptive background mixture model for real-time tracking with shadow detection" by P. KadewTraKuPong and R. Bowden in 2001. It uses a method to model each background pixel by a mixture of K Gaussian distributions (K = 3 to 5). The weights of the mixture represent the time proportions that those colours stay in the scene. The probable background colours are the ones which stay longer and are more static.

While coding, we need to create a background object using the function, **cv2.createBackgroundSubtractorMOG()**. It has some optional parameters like: length of history, number of gaussian mixtures, threshold etc. These are all set to some default values. Then, inside the video loop, use the ``backgroundsubtractor.apply()`` method to get the foreground mask.

See a simple example below:
::

    import numpy as np
    import cv2

    cap = cv2.VideoCapture('vtest.avi')

    fgbg = cv2.createBackgroundSubtractorMOG()

    while(1):
        ret, frame = cap.read()

        fgmask = fgbg.apply(frame)
        
        cv2.imshow('frame',fgmask)
        k = cv2.waitKey(30) & 0xff
        if k == 27:
            break
            
    cap.release()
    cv2.destroyAllWindows()
    
    
( All the results are shown at the end for comparison).


BackgroundSubtractorMOG2
------------------------------------

BackgroundSubtractorMOG2 is also a Gaussian Mixture-based Background/Foreground Segmentation Algorithm. It is based on two papers by Z.Zivkovic, "Improved adaptive Gausian mixture model for background subtraction" in 2004 and "Efficient Adaptive Density Estimation per Image Pixel for the Task of Background Subtraction" in 2006. One important feature of this algorithm is that it selects the appropriate number of gaussian distributions for each pixel. (Remember, in the last case, we took K gaussian distributions throughout the algorithm). It provides better adaptibility to varying scenes due illumination changes etc. 

As in previous case, we have to create a background subtractor object. Here, you have an option of selecting whether the shadow should be detected or ignored. If ``detectShadows = True`` (which is True by default), it detects and marks shadows, but decreases the speed. Shadows will be marked in gray color. 
::

    import numpy as np
    import cv2

    cap = cv2.VideoCapture('vtest.avi')

    fgbg = cv2.createBackgroundSubtractorMOG2()

    while(1):
        ret, frame = cap.read()

        fgmask = fgbg.apply(frame)
        
        cv2.imshow('frame',fgmask)
        k = cv2.waitKey(30) & 0xff
        if k == 27:
            break
            
    cap.release()
    cv2.destroyAllWindows()
    
(Results given at the end)


BackgroundSubtractorGMG
-----------------------------------

This algorithm combines statistical background image estimation and per-pixel Bayesian segmentation. It was introduced by Andrew B. Godbehere, Akihiro Matsukawa, and Ken Goldberg in their paper "Visual Tracking of Human Visitors under Variable-Lighting Conditions for a Responsive Audio Art Installation" in 2012. As per the paper, the system ran a successful interactive audio art installation called “Are We There Yet?” from March 31 - July 31 2011 at the Contemporary Jewish Museum in San Francisco, California. 

It uses the first few (120 by default) frames for background modelling. It employs a probabilistic foreground segmentation algorithm that identifies possible foreground objects using Bayesian inference. The estimates are adaptive; newer observations are more heavily weighted than old observations to accommodate variable illumination. Several morphological filtering operations like closing and opening are done to remove unwanted noise. You will get a black window during first few frames.

It would be better to apply morphological opening to the result to remove the noises.
::

    import numpy as np
    import cv2

    cap = cv2.VideoCapture('vtest.avi')

    kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE,(3,3))
    fgbg = cv2.createBackgroundSubtractorGMG()

    while(1):
        ret, frame = cap.read()

        fgmask = fgbg.apply(frame)
        fgmask = cv2.morphologyEx(fgmask, cv2.MORPH_OPEN, kernel)
        
        cv2.imshow('frame',fgmask)
        k = cv2.waitKey(30) & 0xff
        if k == 27:
            break
            
    cap.release()
    cv2.destroyAllWindows()
    

Results
===========


**Original Frame**

Below image shows the 200th frame of a video

    .. image:: images/resframe.jpg
        :alt: Original frame
        :align: center
        
**Result of BackgroundSubtractorMOG**
        
    .. image:: images/resmog.jpg
        :alt: Result of BackgroundSubtractorMOG
        :align: center
        
**Result of BackgroundSubtractorMOG2**  

Gray color region shows shadow region.
      
    .. image:: images/resmog2.jpg
        :alt: Result of BackgroundSubtractorMOG2
        :align: center
        
**Result of BackgroundSubtractorGMG**

Noise is removed with morphological opening.
      
    .. image:: images/resgmg.jpg
        :alt: Result of BackgroundSubtractorGMG
        :align: center
        
        
Additional Resources
=============================


Exercises
=================
