.. _Matcher:


Feature Matching 
*********************************************

Goal
=====
In this chapter
    * We will see how to match features in one image with others.
    * We will use the Brute-Force matcher and FLANN Matcher in OpenCV
    

Basics of Brute-Force Matcher
===================================

Brute-Force matcher is simple. It takes the descriptor of one feature in first set and is matched with all other features in second set using some distance calculation. And the closest one is returned.

For BF matcher, first we have to create the BFMatcher object using **cv2.BFMatcher()**. It takes two optional params. First one is ``normType``. It specifies the distance measurement to be used. By default, it is ``cv2.NORM_L2``. It is good for SIFT, SURF etc (``cv2.NORM_L1`` is also there). For binary string based descriptors like ORB, BRIEF, BRISK etc, ``cv2.NORM_HAMMING`` should be used, which used Hamming distance as measurement. If ORB is using ``VTA_K == 3 or 4``, ``cv2.NORM_HAMMING2`` should be used.

Second param is boolean variable, ``crossCheck`` which is false by default. If it is true, Matcher returns only those matches with value (i,j) such that i-th descriptor in set A has j-th descriptor in set B as the best match and vice-versa. That is, the two features in both sets should match each other. It provides consistant result, and is a good alternative to ratio test proposed by D.Lowe in SIFT paper.

Once it is created, two important methods are *BFMatcher.match()* and *BFMatcher.knnMatch()*. First one returns the best match. Second method returns `k` best matches where k is specified by the user. It may be useful when we need to do additional work on that. 

Like we used cv2.drawKeypoints() to draw keypoints, **cv2.drawMatches()** helps us to draw the matches. It stacks two images horizontally and draw lines from first image to second image showing best matches.

Let's see one example for each of SURF and ORB (Both use different distance measurements).



Feature Matching with ORB Descriptors
========================================

Here, we will see a simple example on how to match features between two images. In this case, I have a queryImage and a trainImage. We will try to find the queryImage in trainImage using feature matching. ( The images are ``/samples/c/box.png`` and ``/samples/c/box_in_scene.png``)

We are using SIFT descriptors to match features. So let's start with loading images, finding descriptors etc.
::

    import numpy as np
    import cv2
    from matplotlib import pyplot as plt

    img1 = cv2.imread('box.png',0)          # queryImage
    img2 = cv2.imread('box_in_scene.png',0) # trainImage

    # Initiate SIFT detector
    orb = cv2.ORB()

    # find the keypoints and descriptors with SIFT
    kp1, des1 = orb.detectAndCompute(img1,None)
    kp2, des2 = orb.detectAndCompute(img2,None)


Next we create a BFMatcher object with distance measurement ``cv2.NORM_HAMMING`` (since we are using ORB) and ``crossCheck`` is switched on for better results. Then we use Matcher.match() method to get the best matches in two images. We sort them in ascending order of their distances so that best matches (with low distance) come to front. Then we draw only first 10 matches (Just for sake of visibility. You can increase it as you like)
::

    # create BFMatcher object
    bf = cv2.BFMatcher(cv2.NORM_HAMMING, crossCheck=True)

    # Match descriptors.
    matches = bf.match(des1,des2)

    # Sort them in the order of their distance.
    matches = sorted(matches, key = lambda x:x.distance)

    # Draw first 10 matches.
    img3 = cv2.drawMatches(img1,kp1,img2,kp2,matches[:10], flags=2)

    plt.imshow(img3),plt.show()
    
Below is the result I got:

    .. image:: images/matcher_result1.jpg
        :alt: ORB Feature Matching with Brute-Force
        :align: center
        

What is this Matcher Object?
-----------------------------------

The result of ``matches = bf.match(des1,des2)`` line is a list of DMatch objects. This DMatch object has following attributes:

    * ``DMatch.distance`` - Distance between descriptors. The lower, the better it is.
    * ``DMatch.trainIdx`` - Index of the descriptor in train descriptors
    * ``DMatch.queryIdx`` - Index of the descriptor in query descriptors
    * ``DMatch.imgIdx``   - Index of the train image.
    
    
Feature Matching with SIFT Descriptors and Ratio Test
=======================================================

This time, we will use ``BFMatcher.knnMatch()`` to get k best matches. In this example, we will take k=2 so that we can apply ratio test explained by D.Lowe in his paper. 
::

    import numpy as np
    import cv2
    from matplotlib import pyplot as plt

    img1 = cv2.imread('box.png',0)          # queryImage
    img2 = cv2.imread('box_in_scene.png',0) # trainImage

    # Initiate SIFT detector
    sift = cv2.SIFT()

    # find the keypoints and descriptors with SIFT
    kp1, des1 = sift.detectAndCompute(img1,None)
    kp2, des2 = sift.detectAndCompute(img2,None)

    # BFMatcher with default params
    bf = cv2.BFMatcher()
    matches = bf.knnMatch(des1,des2, k=2)

    # Apply ratio test
    good = []
    for m,n in matches:
        if m.distance < 0.75*n.distance:
            good.append([m])

    # cv2.drawMatchesKnn expects list of lists as matches.
    img3 = cv2.drawMatchesKnn(img1,kp1,img2,kp2,good,flags=2)

    plt.imshow(img3),plt.show()

See the result below:

    .. image:: images/matcher_result2.jpg
        :alt: SIFT Descriptor with ratio test
        :align: center
        
        
        
Additional Resources
========================


Exercises
=================
