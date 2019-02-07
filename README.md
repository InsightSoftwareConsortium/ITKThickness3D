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
This contribution implements several filters based on the euclidean distance transform and binary skeleton to fill this blank.



Overview
--------

This is a module for the [Insight Toolkit (ITK)](http://itk.org) that provides filters that compute the skeleton and thickness transforms of a 3D image.


### Binary thinning

The thinning algorithm used in this modules comes from the [Insight Journal article](http://hdl.handle.net/1926/1292):  
```  
  Homann H.  
  Implementation of a 3D thinning algorithm  
  The Insight Journal - 2007 July - December.  
  http://hdl.handle.net/1926/1292  
  http://insight-journal.org/browse/publication/181  
```

### Medial thickness

The idea behind the **medial thickness** as implemented in this module is *the shortest distance from the outside to the object medial axis*.  
This definition provides an *unbiased* method for thickness evaluation compared to the *local thickness transform* since this last one will result in a higher number of points for thick parts. The skeletonization insure a *minimal* set of measurements to fully describe the object.

Filters
-------

- **[`itk::BinaryThinningImageFilter3D<TInputImage, TOutputImage>`](include/itkBinaryThinningImageFilter3D.h)**: Compute the 3D [skeleton](http://hdl.handle.net/1926/1292) of the input image.
-   **[`itk::MedialThicknessImageFilter3D<TInputImage, TOutputImage>`](./include/itkMedialThicknessImageFilter3D.h)**: The [distance](https://itk.org/Doxygen/html/classitk_1_1SignedMaurerDistanceMapImageFilter.html) to the outside along the [medial axis](https://github.com/T4mmi/ITKBinaryThinning3D).


Installation
------------

### Python

Binary [Python packages](https://pypi.python.org/pypi/itk-thickness3d) are available for Linux, macOS, and Windows. They can be installed with:
```
    python -m pip install --upgrade pip
    python -m pip install itk-thickness3d
```

### C++

You need to [build ITK](https://itk.org/Wiki/ITK/Configuring_and_Building) from source to use this module.

In the newer versions of ITK, this module is available as a [*Remote*](https://blog.kitware.com/advance-itk-with-modules/) module in the ITK source code. Build it with the `CMake` option: `Module_Thickness3D`, this can be switched on with a `CMake` graphical interface `ccmake` or directly from the command line with: `-DModule_Thickness3D:BOOL=ON`

For older ITK versions, add it manually as an *External* or *Remote* module to the ITK source code:

External:
```
cd ${ITK_SOURCE_CODE}/Modules/External
git clone https://github.com/InsightSoftwareConsortium/ITKThickness3D
```
Remote:  
create a file in ${ITK_SOURCE_CODE}/Modules/Remote called [Thickness3D.remote.cmake](https://gist.github.com/T4mmi/20449a97dce99f71eec8a9bb6e8853d4) with the content:
```
itk_fetch_module(Thickness3D
  "Tools for 3D thickness measurement"
  GIT_REPOSITORY ${git_protocol}://github.com/T4mmi/ITKBinaryThinning3D.git
  GIT_TAG 67fe3aeb6c5c131b5d364b67565e8d895500e1e3
)
```

Usage
-----

### Python

Once ITK imported, you can use the `itk.MedialThicknessImageFilter3D` just as any other [ITK methods](https://itkpythonpackage.readthedocs.io/en/latest/Quick_start_guide.html):
```
skeleton = itk.BinaryThinningImageFilter3D.New(image)
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
