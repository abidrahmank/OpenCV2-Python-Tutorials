.. _Filtering:

Smoothing Images
***********************

Goals
=======

Learn to:
    * Blur  imagess with various low pass filters
    * Apply custom-made filters to images (2D convolution)
    
2D Convolution ( Image Filtering )
====================================

As for one-dimensional signals, images also can be filtered with various low-pass filters (LPF), high-pass filters (HPF), etc. A LPF helps in removing noise, or blurring the image. A HPF filters helps in finding edges in an image.

OpenCV provides a function, **cv2.filter2D()**, to convolve a kernel with an image. As an example, we will try an averaging filter on an image. A 5x5 averaging filter kernel can be defined as follows:

.. math::

    K =  \frac{1}{25} \begin{bmatrix} 1 & 1 & 1 & 1 & 1  \\ 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 \end{bmatrix}
    
Filtering with the above kernel results in the following being performed: for each pixel, a 5x5 window is centered on this pixel,  all pixels falling within this window are summed up, and the result is then divided by 25. This equates to computing the average of the pixel values inside that window. This operation is performed for all the pixels in the image to produce the output filtered image. Try this code and check the result:
::

    import cv2
    import numpy as np
    from matplotlib import pyplot as plt

    img = cv2.imread('opencv_logo.png')

    kernel = np.ones((5,5),np.float32)/25
    dst = cv2.filter2D(img,-1,kernel)

    plt.subplot(121),plt.imshow(img),plt.title('Original')
    plt.xticks([]), plt.yticks([])
    plt.subplot(122),plt.imshow(dst),plt.title('Averaging')
    plt.xticks([]), plt.yticks([])
    plt.show()
    
Result:

    .. image:: images/filter.jpg
        :alt: Averaging Filter
        :align: center    
    
Image Blurring (Image Smoothing)
==================================

Image blurring is achieved by convolving the image with a low-pass filter kernel. It is useful for removing noise. It actually removes high frequency content (e.g: noise, edges) from the image resulting in edges being blurred when this is filter is applied. (Well, there are blurring techniques which do not blur edges). OpenCV provides mainly four types of blurring techniques.

1. Averaging
--------------

This is done by convolving the image with a normalized box filter. It simply takes the average of all the pixels under kernel area and replaces the central element with this average. This is done by the function **cv2.blur()** or **cv2.boxFilter()**. Check the docs for more details about the kernel. We should specify the width and height of kernel. A 3x3 normalized box filter would look like this:

.. math::

        K =  \frac{1}{9} \begin{bmatrix} 1 & 1 & 1  \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{bmatrix}

.. note:: If you don't want to use a normalized box filter, use **cv2.boxFilter()** and pass the argument ``normalize=False`` to the function.

Check the sample demo below with a kernel of 5x5 size:
::

    import cv2
    import numpy as np
    from matplotlib import pyplot as plt

    img = cv2.imread('opencv_logo.png')

    blur = cv2.blur(img,(5,5))

    plt.subplot(121),plt.imshow(img),plt.title('Original')
    plt.xticks([]), plt.yticks([])
    plt.subplot(122),plt.imshow(blur),plt.title('Blurred')
    plt.xticks([]), plt.yticks([])
    plt.show()
    
Result:

    .. image:: images/blur.jpg
        :alt: Averaging Filter
        :align: center


2. Gaussian Filtering
----------------------

In this approach, instead of a box filter consisting of equal filter coefficients, a Gaussian kernel is used. It is done with the function, **cv2.GaussianBlur()**. We should specify the width and height of the kernel which should be positive and odd. We also should specify the standard deviation in the X and Y directions, sigmaX and sigmaY respectively. If only sigmaX is specified, sigmaY is taken as equal to sigmaX. If both are given as zeros, they are calculated from the kernel size. Gaussian filtering is highly effective in removing Gaussian noise from the image.

If you want, you can create a Gaussian kernel with the function, **cv2.getGaussianKernel()**.

The above code can be modified for Gaussian blurring:
::

    blur = cv2.GaussianBlur(img,(5,5),0)


Result:

    .. image:: images/gaussian.jpg
        :alt: Gaussian Blurring
        :align: center


3. Median Filtering
--------------------

Here, the function **cv2.medianBlur()** computes the median of all the pixels under the kernel window and the central pixel is replaced with this median value. This is highly effective in removing salt-and-pepper noise. One interesting thing to note is that, in the Gaussian and box filters, the filtered value for the central element can be a value which may not exist in the original image. However this is not the case in median filtering, since the central element is always replaced by some pixel value in the image. This reduces the noise effectively. The kernel size must be a positive odd integer.

In this demo, we add a 50% noise to our original image and use a median filter. Check the result:
::

    median = cv2.medianBlur(img,5)
    
Result:

    .. image:: images/median.jpg
        :alt: Median Blurring
        :align: center


4. Bilateral Filtering
-----------------------

As we noted, the filters we presented earlier tend to blur edges. This is not the case for the bilateral filter, **cv2.bilateralFilter()**, which was defined for, and is highly effective at noise removal while preserving edges. But the operation is slower compared to other filters. We already saw that a Gaussian filter takes the a neighborhood around the pixel and finds its Gaussian weighted average. This Gaussian filter is a function of space alone, that is, nearby pixels are considered while filtering. It does not consider whether pixels have almost the same intensity value and does not consider whether the pixel lies on an edge or not. The resulting effect is that Gaussian filters tend to blur edges, which is undesirable. 

The bilateral filter also uses a Gaussian filter in the space domain, but it also uses one more (multiplicative) Gaussian filter component which is a function of pixel intensity differences. The Gaussian function of space makes sure that only pixels are 'spatial neighbors' are considered for filtering, while the Gaussian component applied in the intensity domain (a Gaussian function of intensity differences) ensures that only those pixels with intensities similar to that of the central pixel ('intensity neighbors') are included to compute the blurred intensity value. As a result, this method preserves edges, since for pixels lying near edges, neighboring pixels placed on the other side of the edge, and therefore exhibiting large intensity variations when compared to the central pixel, will not be included for blurring.

The sample below demonstrates the use of bilateral filtering (For details on arguments, see the OpenCV docs).
::

    blur = cv2.bilateralFilter(img,9,75,75)
    
Result:
    
    .. image:: images/bilateral.jpg
        :alt: Bilateral Filtering
        :align: center    

Note that the texture on the surface is gone, but edges are still preserved.

Additional Resources
======================

1. Details about the `bilateral filtering can be found at <http://people.csail.mit.edu/sparis/bf_course/>`_

Exercises
===========

Take an image, add Gaussian noise and salt and pepper noise, compare the effect of blurring via box, Gaussian, median and bilateral filters for both noisy images, as you change the level of noise.
