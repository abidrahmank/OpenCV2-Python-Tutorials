.. _Lucas_Kanade:


Optical Flow using Lucas-Kanade Method
*********************************************

Goal
=======

In this chapter,
    * We will understand the concepts of optical flow and its estimation using Lucas-Kanade method.
    * We will use functions like **cv2.calcOpticalFlowPyrLK()** to track feature points in a video.
    

Optical Flow
================

Optical flow is the pattern of apparent motion of image objects between two consecutive frames caused by the movemement of object or camera. It is 2D vector field where each vector is a displacement vector showing the movement of points from first frame to second. Consider the image below (Image Courtesy: `Wikipedia article on Optical Flow <http://en.wikipedia.org/wiki/Optical_flow>`_). 


    .. image:: images/optical_flow_basic1.png
        :alt: Optical Flow
        :align: center

It shows a ball moving in 5 consecutive frames. The arrow shows its displacement vector. Optical flow has many applications in areas like :

    * Structure from Motion
    * Video Compression
    * Video Stabilization ...
    
Optical flow works on several assumptions:

1. The pixel intensities of an object do not change between consecutive frames.
2. Neighbouring pixels have similar motion.

Consider a pixel :math:`I(x,y,t)` in first frame (Check a new dimension, time, is added here. Earlier we were working with images only, so no need of time). It moves by distance :math:`(dx,dy)` in next frame taken after :math:`dt` time. So since those pixels are the same and intensity does not change, we can say,

.. math::

    I(x,y,t) = I(x+dx, y+dy, t+dt)
    
Then take taylor series approximation of right-hand side, remove common terms and divide by :math:`dt` to get the following equation:

.. math::

    f_x u + f_y v + f_t = 0 \; 
    
where:

.. math:: 
        
    f_x = \frac{\partial f}{\partial x} \; ; \; f_y = \frac{\partial f}{\partial x}
    
    u = \frac{dx}{dt} \; ; \; v = \frac{dy}{dt}


Above equation is called Optical Flow equation. In it, we can find :math:`f_x` and :math:`f_y`, they are image gradients. Similarly :math:`f_t` is the gradient along time. But :math:`(u,v)` is unknown. We cannot solve this one equation with two unknown variables. So several methods are provided to solve this problem and one of them is Lucas-Kanade.

Lucas-Kanade method
-------------------------

We have seen an assumption before, that all the neighbouring pixels will have similar motion. Lucas-Kanade method takes a 3x3 patch around the point. So all the 9 points have the same motion. We can find :math:`(f_x, f_y, f_t)` for these 9 points. So now our problem becomes solving 9 equations with two unknown variables which is over-determined. A better solution is obtained with least square fit method. Below is the final solution which is two equation-two unknown problem and solve to get the solution.

.. math::

    \begin{bmatrix} u \\ v \end{bmatrix} = 
    \begin{bmatrix} 
        \sum_{i}{f_{x_i}}^2  &  \sum_{i}{f_{x_i} f_{y_i} } \\
        \sum_{i}{f_{x_i} f_{y_i}} & \sum_{i}{f_{y_i}}^2 
    \end{bmatrix}^{-1}
    \begin{bmatrix} 
        - \sum_{i}{f_{x_i} f_{t_i}} \\
        - \sum_{i}{f_{y_i} f_{t_i}} 
    \end{bmatrix}
    
    
( Check similarity of inverse matrix with Harris corner detector. It denotes that corners are better points to be tracked.)

So from user point of view, idea is simple, we give some points to track, we receive the optical flow vectors of those points. But again there are some problems. Until now, we were dealing with small motions. So it fails when there is large motion. So again we go for pyramids. When we go up in the pyramid, small motions are removed and large motions becomes small motions. So applying Lucas-Kanade there, we get optical flow along with the scale. 


Lucas-Kanade Optical Flow in OpenCV
=======================================

OpenCV provides all these in a single function, **cv2.calcOpticalFlowPyrLK()**. Here, we create a simple application which tracks some points in a video. To decide the points, we use **cv2.goodFeaturesToTrack()**. 
