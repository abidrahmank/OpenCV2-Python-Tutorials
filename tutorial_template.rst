.. _Tutorial-Template:

Title of Doc comes here
***********************

Goals
======

Explain here following things:

.. container:: enumeratevisibleitemswithsquare

    * What is this tutorial about ? What you will learn here?
    * What are all new functions you will see here (eg :ocv:func:`borderInterpolate`) 
    
Theory
======

.. container:: enumeratevisibleitemswithsquare

    * Some intuitive explanation of algorithm is given here
    * If needed, give some equations as inline :math:`g(i,j)` or next line as,
    
    .. math::
        g(i,h) = \sum_{k,l} f(i+k, j+l) h(k,l)
        
    * If required, a Numpy implementation of algorithm also can be given as a separate subsection
    
Subsection Python Implementation [optional]
--------------------------------------------

Numpy code comes here. To add code, do as follows :
::
    import cv2
    import numpy as np
    
    print 'import done'
    
OpenCV sample
=============

Here comes the original OpenCV code with explanation. Result also can included in this itself.

To add image, do as follows :

     .. image:: images/messi5.jpg
              :alt: Smoothing with a median filter
              :align: center    

Notes, warnings, Todo etc can be done as follows :

.. note::
   The explanation below belongs to the book
.. warning::
   The explanation below belongs to the book
.. todo::
   The explanation below belongs to the book    
.. seealso::
   The explanation below belongs to the book  
              
external urls are given as `Python <http://www.python.org>`_ which points to python site.

Internal url is called as Tutorial-Template_

A book is cited as [Szeliski]_
          
Common Errors [optional]     
========================

We can show solutions for some common mistakes while using certain functionalities, if any.  
            
Exercises [optional]
=====================

Here we can give some additional tasks for reader to do 

    * like read and understand more advanced code on the same algorithm.
    * Related SOF and answers.opencv.org questions
    * Our own questions or tasks
    
Additional Resources [optional]
================================

Give references if any for better understanding of algorithm, like any standard textbooks, web links etc. Numbered references are given as 

#. Learning OpenCV 
#. Computer Vision Models
   
   
.. [Szeliski] Computer Vision Models              
