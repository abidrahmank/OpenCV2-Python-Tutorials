.. _Display_Image:

Getting Started with Images
*****************************

Goals
======

.. container:: enumeratevisibleitemswithsquare

    * Here, you will learn how to read an image, display it and save it
    * You will learn these functions : **cv2.imread()**, **cv2.imshow()** , **cv2.imwrite()**
    * Optionally, you will learn how to display images with Matplotlib

Using OpenCV
=============

Read an image
--------------

Use the function **cv2.imread()** to read an image. The image should be in the working directory, or the full path of the image should be given.

The second argument is a flag which specifies the way image should be read.

* cv2.IMREAD_COLOR : Loads a color image. Transparency will be neglected. This is the default flag.
* cv2.IMREAD_GRAYSCALE : Loads image in grayscale mode.
* cv2.IMREAD_UNCHANGED : Loads image as such including alpha channel (transparency).

See the code below:
::
    
    import numpy as np
    import cv2
    
    # Load an color image in grayscale
    img = cv2.imread('messi5.jpg', cv2.IMREAD_GRAYSCALE)
    
.. warning:: If the image path is wrong, this won't throw any error, but ``print img`` will return ``None``

Display an image
-----------------

Use the function **cv2.imshow()** to display an image in a window. The window automatically fits to the image size.

The first argument is a window name which is a string. The second argument is our image. You can create as many windows as you wish, by using different window names.
::
    
    cv2.imshow('image',img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()

The window will look like this (on a Fedora-Gnome machine):

     .. image:: images/opencv_screenshot.jpg
              :alt: Screenshot of Image Window in OpenCV
              :align: center 
   
**cv2.waitKey()** is a keyboard binding function that waits for any keyboard event. If any key is pressed, the program continues. Its argument is the wait time in milliseconds. If **0** is passed, it waits indefinitely for a key stroke. It can also be set to detect specific key strokes like, if key `a` is pressed (see below).

**cv2.destroyAllWindows()** destroys all the windows we created. If you want to destroy a specific window, use the function **cv2.destroyWindow()** where you pass the exact window name as the argument.

.. note:: There is a special case where you can already create a window and load image to it later. In that case, you can specify whether the window is resizable or not, by using the function **cv2.namedWindow()**. By default, the flag is ``cv2.WINDOW_AUTOSIZE``. But if you specify the flag to be ``cv2.WINDOW_NORMAL``, you can resize the window. This is helpful for large images that need scrollbars.

See the code below:
::
    
    cv2.namedWindow('image', cv2.WINDOW_NORMAL)
    cv2.imshow('image',img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
    
Write an image
---------------

Use the function **cv2.imwrite()** to save an image.

The first argument is the file name, the second argument is the image you want to save.
::
    
    cv2.imwrite('messigray.png',img)

This will save the image in PNG format in the working directory. 

Sum it up
---------------

Below program loads an image in grayscale, displays it, save the image if you press 's' and exit, or simply exit without saving if you press `ESC` key.
::
    
    import numpy as np
    import cv2
    
    img = cv2.imread('messi5.jpg', cv2.IMREAD_GRAYSCALE)
    cv2.imshow('image',img)
    k = cv2.waitKey(0)
    if k == 27:         # wait for ESC key to exit
        cv2.destroyAllWindows()
    elif k == ord('s'): # wait for 's' key to save and exit
        cv2.imwrite('messigray.png',img)
        cv2.destroyAllWindows()
    
.. warning:: If you are using a 64-bit machine, you will have to modify the line ``k = cv2.waitKey(0)`` as follows : ``k = cv2.waitKey(0) & 0xFF``

Using Matplotlib
=================

Matplotlib is a plotting library for Python that offers a wide variety of plotting methods. Here, you will learn how to display an image with Matplotlib. With it, you can zoom into images, save them, and more.
::
    
    import numpy as np
    import cv2
    from matplotlib import pyplot as plt
    
    img = cv2.imread('messi5.jpg', cv2.IMREAD_GRAYSCALE)
    plt.imshow(img, cmap = 'gray', interpolation = 'bicubic')
    plt.xticks([]), plt.yticks([])  # to hide tick values on X and Y axis
    plt.show()
    
The window will look like this :

     .. image:: images/matplotlib_screenshot.jpg
              :alt: Screenshot of Image Window in Matplotlib
              :align: center 
    
.. seealso:: Plenty of plotting options are available in Matplotlib. Please refer to the Matplotlib docs for more details. Some of them will be demonstrated within these tutorials.

.. warning:: Color image loaded by OpenCV is in BGR mode. But Matplotlib displays in RGB mode. So color images will not be displayed correctly in Matplotlib if image is read with OpenCV. Please see the exercises for more details.

Additional Resources
======================

#. `Matplotlib Plotting Styles and Features <http://matplotlib.org/api/pyplot_api.html>`_

Exercises
==========

#. There can be problems when you try to load a color image in OpenCV and display it in Matplotlib. Read `this discussion <http://stackoverflow.com/a/15074748/1134940>`_ for details.
