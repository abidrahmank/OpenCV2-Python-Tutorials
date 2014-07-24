.. _Bindings_Basics:

How OpenCV-Python Bindings Works?
************************************

Goal
=====

Learn:
    
    * How OpenCV-Python bindings are generated?
    * How to extend new OpenCV modules to Python?

How OpenCV-Python bindings are generated?
=========================================

In OpenCV, all algorithms are implemented in C++. But these algorithms can be used from different languages like Python, Java etc. This is made possible by the bindings generator files. These files form a bridge between C++ and Python which enable users to call C++ functions from Python. To get a complete picture of what is happening background, a good knowledge of Python/C API is required. A simple example on extending C++ functions to Python can be found in official Python documentation[1]. So extending every function in OpenCV to Python by writing its wrapper function is a time-consuming task. So OpenCV does it more intelligent way. OpenCV generates these wrapper functions automatically from the C++ headers using some Python scripts which are located in ``modules/python/src2``. We will look into what they do.

First, ``modules/python/CMakeFiles.txt`` is a cmake script which checks the modules to be extended to Python. It will automatically check all the modules to be extended and grab their header files. These header files contain list of all classes, functions, constants etc. for that particular modules.

Second, these header files are passed to a Python script, ``modules/python/src2/gen2.py``. He is the Python bindings generator script. It calls another Python script ``modules/python/src2/hdr_parser.py``. This is the header parser file. This guy splits the complete header file into small Python lists. So these lists contain all details about a particular function, class etc. For example, a function will be parsed to get a list containing function name, return type, input arguments, argument types etc. Final list contains details of all the functions, structs, classes etc. in that header file.

But header parser doesn't parse all the functions/classes in the header file. Instead, there are certain macros added to the beginning of these declarations which enables the header parser to identify functions to be parsed. These macros are added by the developer who programs the particular function. So the developer decides which functions should be extended to Python and which are not. Details of the macros will be given in next session.

So header parser returns a final big list of parsed functions. Our generator script (gen2.py) will create wrapper functions for all the functions/classes/enums/structs parsed by header parser (You can find these header files during compilation in the ``build/modules/python/`` folder as ``pyopencv_generated_*.h`` files). But there may be some basic OpenCV datatypes like Mat, Vec4i, Size. They need to be extended manually. For example, a Mat type should be extended to Numpy array, Size should be extended to a tuple of two integers etc. Similarly, there may be some complex structs/classes/functions etc. which need to be extended manually. All such manual wrapper functions are placed in ``modules/python/src2/pycv2.hpp``.

So now only thing left is the compilation of these wrapper files which gives us **cv2** module. So when you call a function, say ``res = equalizeHist(img1,img2)`` in Python, ``cv::Mat`` structures corresponding to ``img1``, ``img2`` and ``res`` will be formed and calls the ``equalizeHist()`` function in C++. Final result, ``res`` will be converted back into a Numpy array. So in short, almost all operations are done in C++ which gives us almost same speed as that of C++.

So this is the basic version of how OpenCV-Python bindings are generated.


How to extend new modules to Python?
=====================================

Header parser parse the header files based on some wrapper macros added to function declaration. Enumeration constants don't need any wrapper macros. They are automatically wrapped. But remaining functions, classes etc. need wrapper macros.

Functions are extended using ``CV_EXPORTS_W`` macro. An example is shown below.

.. code-block:: cpp

    CV_EXPORTS_W void equalizeHist( InputArray src, OutputArray dst );

Header parser can understand the input and output arguments from keywords like ``InputArray, OutputArray`` etc. But sometimes, we may need to hard-code inputs and outputs. For that, macros like ``CV_OUT, CV_IN_OUT`` etc. are used.

.. code-block:: cpp

    CV_EXPORTS_W void minEnclosingCircle( InputArray points,
                                         CV_OUT Point2f& center, CV_OUT float& radius );

For large classes also, ``CV_EXPORTS_W`` is used. To extend class methods, ``CV_WRAP`` is used. Similarly, ``CV_PROP`` is used for class fields. 

.. code-block:: cpp

    class CV_EXPORTS_W CLAHE : public Algorithm
    {
    public:
        CV_WRAP virtual void apply(InputArray src, OutputArray dst) = 0;
    
        CV_WRAP virtual void setClipLimit(double clipLimit) = 0;
        CV_WRAP virtual double getClipLimit() const = 0;
    }

Overloaded functions can be extended using ``CV_EXPORTS_AS``. But we need to pass a new name so that each function will be called by that name in Python. Take the case of integral function below. Three functions are available, so each one is named with a suffix in Python. Similarly ``CV_WRAP_AS`` can be used to wrap overloaded methods.

.. code-block:: cpp

    //! computes the integral image
    CV_EXPORTS_W void integral( InputArray src, OutputArray sum, int sdepth = -1 );
    
    //! computes the integral image and integral for the squared image
    CV_EXPORTS_AS(integral2) void integral( InputArray src, OutputArray sum,
                                            OutputArray sqsum, int sdepth = -1, int sqdepth = -1 );
    
    //! computes the integral image, integral for the squared image and the tilted integral image
    CV_EXPORTS_AS(integral3) void integral( InputArray src, OutputArray sum,
                                            OutputArray sqsum, OutputArray tilted,
                                            int sdepth = -1, int sqdepth = -1 );
     
Small classes/structs are extended using ``CV_EXPORTS_W_SIMPLE``. These structs are passed by value to C++ functions. Examples are KeyPoint, Match etc. Their methods are extended by ``CV_WRAP`` and fields are extended by ``CV_PROP_RW``. 

.. code-block:: cpp

    class CV_EXPORTS_W_SIMPLE DMatch
    {
    public:
        CV_WRAP DMatch();
        CV_WRAP DMatch(int _queryIdx, int _trainIdx, float _distance);
        CV_WRAP DMatch(int _queryIdx, int _trainIdx, int _imgIdx, float _distance);
    
        CV_PROP_RW int queryIdx; // query descriptor index
        CV_PROP_RW int trainIdx; // train descriptor index
        CV_PROP_RW int imgIdx;   // train image index
    
        CV_PROP_RW float distance;
    };

Some other small classes/structs can be exported using ``CV_EXPORTS_W_MAP`` where it is exported to a Python native dictionary. Moments() is an example of it.

.. code-block:: cpp

    class CV_EXPORTS_W_MAP Moments
    {
    public:
        //! spatial moments
        CV_PROP_RW double  m00, m10, m01, m20, m11, m02, m30, m21, m12, m03;
        //! central moments
        CV_PROP_RW double  mu20, mu11, mu02, mu30, mu21, mu12, mu03;
        //! central normalized moments
        CV_PROP_RW double  nu20, nu11, nu02, nu30, nu21, nu12, nu03;
    };

So these are the major extension macros available in OpenCV.
