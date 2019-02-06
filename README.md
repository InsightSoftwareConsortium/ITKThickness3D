[CircleCI]: https://img.shields.io/circleci/project/github/T4mmi/ITKThickness3D/master.svg?label=Linux
[TravisCI]: https://img.shields.io/travis/T4mmi/ITKThickness3D/master.svg?label=macOS
[Appveyor]: https://img.shields.io/appveyor/ci/T4mmi/itkthickness3d.svg?label=Windows
[Python version]: https://img.shields.io/pypi/pyversions/itk-thickness3d.svg
[PyPI version]: https://img.shields.io/pypi/v/itk-thickness3d.svg?label=PyPI

ITKThickness3D
==============

**An ITK module to compute 3D thickness** 

![CircleCI][CircleCI] ![TravisCI][TravisCI] ![Appveyor][Appveyor] ![Python version][Python version] ![PyPI version][PyPI version] 

ITK currently comes without a hands on solution for 3D thickness evaluation.  
This contribution implements a composite filter based on the euclidean distance transform and binary skeleton to fill this blank.


Overview
--------

This is a module for the [Insight Toolkit (ITK)](http://itk.org) that provides filters that compute the thickness of a 3D image. It is build on top of [ITKBinaryThinning3D](https://github.com/T4mmi/ITKBinaryThinning3D).

The idea behind the **medial thickness** as implemented in this module is *the shortest distance from the outside to the object medial axis*. The **local thickness** is the medial thickness retropropagated to the neighbors (*work in progress*).


### Filters
-   **[`itk::MedialThicknessImageFilter3D<TInputImage, TOutputImage>`](./include/itkMedialThicknessImageFilter3D.h)**: The [distance](https://itk.org/Doxygen/html/classitk_1_1SignedMaurerDistanceMapImageFilter.html) to the outside along the [medial axis](https://github.com/T4mmi/ITKBinaryThinning3D).
-   **`itk::LocalThicknessImageFilter3D<TInputImage, TOutputImage>`**: *- Work In Progress -* The local thickness filter.


Installation
------------

Binary [Python packages](https://pypi.python.org/pypi/itk-thickness3d) are available for Linux, macOS, and Windows. They can be installed with:
```
    python -m pip install --upgrade pip
    python -m pip install itk-thickness3d
```


Usage
-----
Once ITK imported, you can use the `itk.MedialThicknessImageFilter3D` just as any other [ITK methods](https://itkpythonpackage.readthedocs.io/en/latest/Quick_start_guide.html):
```
thickness_map = itk.MedialThicknessImageFilter3D.New(image)
```  

Here is a simple python script that reads an image, applies the medial thhickness filter and writes the resulting image in a file:
```
import itk

input_filename = sys.argv[1]
output_filename = sys.argv[2]

image = itk.imread(input_filename)
thickness_map = itk.MedialThicknessImageFilter3D.New(image)

itk.imwrite(thickness_map, output_filename)
```


License
-------

This software is distributed under the Apache 2.0 license.  
Please see the *LICENSE* file for details.


Acknowledgements
----------------

See also : [ITKBinaryThinning3D](https://github.com/T4mmi/ITKBinaryThinning3D)

**This is module is still under development !**  
*Do not use in production (yet)*
