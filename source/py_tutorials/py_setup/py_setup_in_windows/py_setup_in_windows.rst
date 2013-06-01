.. _Install-OpenCV-Python-in-Windows:

Install OpenCV-Python in Windows
*********************************

Goals
======

In this tutorial, you will learn to setup OpenCV-Python in your Windows system. Below steps are tested for Windows 7.

Required Packages
==================
#. Python
#. Numpy
#. OpenCV

Below are some optional packages which will be useful in your journey.

#. Matplotlib
#. SciPy

Install Python, Numpy, Matplotlib, Scipy to their default locations. To install OpenCV, you can either use prebuilt binaries or compile from source.

Installing OpenCV from prebuilt binaries
=========================================
#. Goto opencv/../../2.7 folder.
#. Copy cv2.pyd to C:/Python27/lib/site-packeges
#. Open Python IDLE and type following codes in Python terminal.

    >>> import cv2
    >>> print cv2.__version__
    
If the results are printed out without any errors, congratulations !!! You have installed OpenCV-Python successfully.

Installing OpenCV from source 
===============================
#. Put complete compilation steps here


