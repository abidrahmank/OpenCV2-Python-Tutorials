.. _Table-Of-Content-Feature2D:

Feature Detection and Description
------------------------------------------

*  :ref:`Features_Meaning`

  .. tabularcolumns:: m{100pt} m{300pt}
  .. cssclass:: toctableopencv

  =========== ======================================================
  |f2d_1|     What are the main features in an image? How can finding those features be useful to us?

  =========== ======================================================

  .. |f2d_1|  image:: images/test.jpg
                 :height: 90pt
                 :width:  90pt


*  :ref:`Harris_Corners`

  .. tabularcolumns:: m{100pt} m{300pt}
  .. cssclass:: toctableopencv

  =========== ======================================================
  |f2d_2|     Okay, Corners are good features? But how do we find them?

  =========== ======================================================

  .. |f2d_2|  image:: images/test.jpg
                 :height: 90pt
                 :width:  90pt                 


*  :ref:`shi_tomasi`

  .. tabularcolumns:: m{100pt} m{300pt}
  .. cssclass:: toctableopencv

  =========== ======================================================
  |f2d_3|     We will look into Shi-Tomasi corner detection

  =========== ======================================================

  .. |f2d_3|  image:: images/test.jpg
                 :height: 90pt
                 :width:  90pt 


*  :ref:`sift_intro`

  .. tabularcolumns:: m{100pt} m{300pt}
  .. cssclass:: toctableopencv

  =========== ======================================================
  |f2d_4|     Harris corner detect is not enough when scale of image changes. Lowe developed a breakthrough method to find scale-invariant features and it is called SIFT

  =========== ======================================================

  .. |f2d_4|  image:: images/test.jpg
                 :height: 90pt
                 :width:  90pt


*  :ref:`SURF`

  .. tabularcolumns:: m{100pt} m{300pt}
  .. cssclass:: toctableopencv

  =========== ======================================================
  |f2d_5|     SIFT is really good, but not fast enough, so people came up with a speeded-up version called SURF.

  =========== ======================================================

  .. |f2d_5|  image:: images/test.jpg
                 :height: 90pt
                 :width:  90pt   


*  :ref:`FAST`

  .. tabularcolumns:: m{100pt} m{300pt}
  .. cssclass:: toctableopencv

  =========== ======================================================
  |f2d_06|    All the above feature detection methods are good in some way. But they are not fast enough to work in real-time applications like SLAM. There comes the FAST algorithm, which is really "FAST".

  =========== ======================================================

  .. |f2d_06|  image:: images/test.jpg
                 :height: 90pt
                 :width:  90pt   


                 
.. raw:: latex

   \pagebreak

.. We use a custom table of content format and as the table of content only informs Sphinx about the hierarchy of the files, no need to show it.
.. toctree::
   :hidden:

   ../py_features_meaning/py_features_meaning
   ../py_features_harris/py_features_harris
   ../py_shi_tomasi/py_shi_tomasi
   ../py_sift_intro/py_sift_intro
   ../py_surf_intro/py_surf_intro
   ../py_fast/py_fast
