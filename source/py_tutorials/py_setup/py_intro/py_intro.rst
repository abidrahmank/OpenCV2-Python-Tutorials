.. _Intro:


Introduction to OpenCV-Python Tutorials
*******************************************

OpenCV
===============

OpenCV was started at Intel in 1999 by **Gary Bradsky** and the first release came out in 2000. **Vadim Pisarevsky** joined Gary Bradsky to manage Intel's Russian software OpenCV team. In 2005, OpenCV was used on Stanley, the vehicle who won 2005 DARPA Grand Challenge. Later its active development continued under the support of Willow Garage, with Gary Bradsky and Vadim Pisarevsky leading the project. Right now, OpenCV supports a lot of algorithms related to Computer Vision and Machine Learning and it is expanding day-by-day.

Currently OpenCV supports a wide variety of programming languages like C++, Python, Java etc and is available on different platforms including Windows, Linux, OS X, Android, iOS etc. Also, interfaces based on CUDA and OpenCL are also under active development for high-speed GPU operations.

OpenCV-Python is the Python API of OpenCV. It combines the best qualities of OpenCV C++ API and Python language. 


OpenCV-Python
===============

Python is a general purpose interpreted programming language started by **Guido van Rossum** in 1991. It became very popular in short time mainly because of its simplicity and code readability. It enables the programmer to express his ideas in fewer lines of code without reducing readability.

Compared to compiled languages Python is slower, but it can be easily extended with C/C++. This feature allows computationally intensive code to be written in C/C++, put inside a Python wrapper and imported as a Python module. This provides run speeds as fast as original C/C++ code (since the actual C++ code is running in background) while retaining the simplicity of python code. OpenCV-Python is a Python wrapper around the original C++ implementation of OpenCV.

OpenCV also takes advantage of Numpy. **Numpy** is a highly optimized library for numerical operations with a MATLAB-style syntax. All OpenCV array structures are converted to-and-from Numpy arrays and support Numpy operations. Numpy integration allows easy interaction with other libraries like, SciPy and Matplotlib, which support Numpy.

OpenCV-Python is an appropriate tool for fast prototyping of computer vision problems.


OpenCV-Python Tutorials
=============================

OpenCV introduces a new set of tutorials which will guide you through various functions available in OpenCV-Python. **This guide is focused on OpenCV 3.x** (although most of the tutorials will work with OpenCV 2.x also).

Prior knowledge on Python and Numpy is required before starting because they won't be covered in this guide. **A good understanding of Numpy is a must to write optimized code in OpenCV-Python.**

This tutorial has been started by *Abid Rahman K.* as part of Google Summer of Code 2013 program, under the guidance of *Alexander Mordvintsev*.


OpenCV Needs You!
==========================

OpenCV is an open source initiative, and all are welcome to make contributions to the library and this tutorial.

If you find any mistakes in this tutorial, please help us correct them! Just fork OpenCV on Github, make the necessary corrections and submit a pull request. OpenCV developers will check your pull request, provide feedback and once approved by a reviewer, it will be merged into OpenCV. Then your name will be credited as a open source contributor on this project.

As new modules are added to OpenCV-Python, this tutorial will need to be expanded. If you know about particular algorithm, please write up a tutorial which includes the basic theory of the algorithm and example code showing basic usage and submit it to OpenCV.

Together, can make this project a great success!


Contributors
=================

Below is the list of contributors who submitted tutorials to OpenCV-Python.

1. Alexander Mordvintsev (GSoC-2013 mentor)
2. Abid Rahman K. (GSoC-2013 intern)


Additional Resources
=======================

1. A Quick guide to Python - `A Byte of Python <http://swaroopch.com/notes/python/>`_
2. `Basic Numpy Tutorials <http://wiki.scipy.org/Tentative_NumPy_Tutorial>`_
3. `Numpy Examples List <http://wiki.scipy.org/Numpy_Example_List>`_
4. `OpenCV Documentation <http://docs.opencv.org/>`_
5. `OpenCV Forum <http://answers.opencv.org/questions/>`_
