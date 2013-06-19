.. _Watershed:

Image Segmentation with Watershed Algorithm
*********************************************

Goal
=====

In this chapter,
    * We will learn to use marker-based image segmentation using watershed algorithm
    * We will see: **cv2.watershed()**
    
Theory
========

Any grayscale image can be viewed as a topographic surface where high intensity denotes peaks and hills while low intensity denotes valleys. You start filling every isolated valleys (local minima) with different colored water (labels). As the water rises, depending on the peaks (gradients) nearby, water from different valleys, obviously with different colors will start to merge. To avoid that, you build barriers in the locations where water merges. You continue the work of filling water and building barriers until all the peaks are under water. Then the barriers you created gives you the segmentation result. This is the "philosophy" behind the watershed. You can visit the `CMM webpage on watershed <http://cmm.ensmp.fr/~beucher/wtshed.html>`_ to understand it with the help of some animations. 

But this approach gives you oversegmented result due to noise or any other irregularities in the image. So OpenCV implemented a marker-based watershed algorithm where you specify which are all valley points are to be merged and which are not. It is an interactive image segmentation. What we do is to give different labels for our object we know. Label the region which we are sure of being the foreground or object with one color (or intensity), label the region which we are sure of being background or non-object with another color and finally the region which we are not sure of anything, label it with 0. That is out marker. Then apply watershed algorithm. Then our marker will be updated with the labels we gave, and the boundaries of objects will have a value of -1. 

Code
========

Below we will see an example on how to use the Distance Transform along with watershed to segment mutually touching objects.

Consider the coins image below, the coins are touching each other. Even if you threshold it, it will be touching each other. 

    .. image:: images/water_coins.jpg
        :alt: Coins
        :align: center
    
We start with finding an approximate estimate of the coins. For that, we can use the Otsu's binarization.
::

    import numpy as np
    import cv2
    from matplotlib import pyplot as plt

    img = cv2.imread('coins.png')
    gray = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
    ret, thresh = cv2.threshold(gray,0,255,cv2.THRESH_BINARY_INV+cv2.THRESH_OTSU)

Result:

    .. image:: images/water_thresh.jpg
        :alt: Thresholding
        :align: center
        
Now we need to remove any small white noises in the image. For that we can use morphological opening. To remove any small holes in the object, we can use morphological closing. So, now we know for sure that region near to center of objects are foreground and region much away from the object are background. Only region we are not sure is the boundary region of coins. 

So we need to extract the area which we are sure they are coins. Erosion removes the boundary pixels. So whatever remaining we get, we can be sure it is coin. Then we need to find the area which we are sure they are not coins. For that, we dilate the result. Dilation increases object boundary to background. This way, we can make sure whatever region in background in result is really a background, since boundary region is removed.
::

    kernel = np.ones((3,3),np.uint8)
    opening = cv2.morphologyEx(thresh,cv2.MORPH_OPEN,kernel, iterations = 2)

    sure_fg = cv2.erode(opening,kernel,iterations=3)
    sure_bg = cv2.dilate(opening,kernel,iterations=3)

See the result to understand what we did:

    .. image:: images/water_fgbg.png
        :alt: Background and Foreground Regions
        :align: center        

The coins are touching. As long as they are touching we can't label two coins as different. So for this purpose, we apply distance transform first, then apply a suitable threshold so that all coins are detached.
::

    dist_transform = cv2.distanceTransform(opening,cv2.cv.CV_DIST_L2,5)
    ret, thresh = cv2.threshold(dist_transform,0.7*dist_transform.max(),255,0)
    
See the result. In the thresholded image, we get some regions of coins which we are sure of coins and they are detached. (In previous step, we have used erosion to find sure coins area, that is not needed in this example. In some cases, you may be interested in only foreground segmentation, not in separating the mutually touching objects. In that case, you need not use distance transform, just erosion is sufficient. Simply, erosion is just another method to extract sure foreground area, that's all.)

    .. image:: images/water_dt.png
        :alt: Distance Transform
        :align: center         


Now we know for sure which are region of coins, which are background and all. So we create marker (it is an array of same size as that of original image, but with int32 datatype) and label the regions inside it. The regions we know for sure are labelled with any positive integers, but different integers, and the area we don't know for sure are just left as zero. For this we use **cv2.findContours()** and draw the contours with different colors. Finally, mark region of sure background with a different color.
::

    markers = np.zeros(gray.shape).astype('int32')
    contours, hierarchy = cv2.findContours(np.uint8(thresh),cv2.RETR_LIST, 1)

    for (i,cnt) in enumerate(contours):
        cv2.drawContours(markers,[cnt],0,i+1,-1)

    markers[sure_bg==0] = i+2
    
See the result shown in JET colormap. The red region shows sure background. Sure coins are colored with different values. Around the coins, you can see an uniform blue color which is the region we are not sure.

    .. image:: images/water_marker.jpg
        :alt: Marker Image
        :align: center  
        
Now our marker is ready. It is time for final step, apply watershed. Then marker image will be modified. The boundary region will be marked with -1.
::

    cv2.watershed(img,markers)
    img[markers == -1] = [255,0,0]
    
See the result below. For some coins, the region where they touch are segmented properly. 

    .. image:: images/water_result.jpg
        :alt: Result
        :align: center
        
Additional Resources
======================
#. CMM page on `Watershed Tranformation <http://cmm.ensmp.fr/~beucher/wtshed.html>`_

Exercises
==============
#. OpenCV samples has an interactive sample on watershed segmentation, `watershed.py`. Run it, Enjoy it, then learn it.

