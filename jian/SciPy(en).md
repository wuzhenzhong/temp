# SciPy

[hzyido](https://www.jianshu.com/u/74b632e3297c)关注

2015.07.24 22:42:35字数 906阅读 159

**From Wikipedia, the free encyclopedia**



Not to be confused with[ScientificPython](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/ScientificPython).

**SciPy**(pronounced “Sigh Pie”) is an[open source](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Open_source)[Python](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Python_(programming_language))library used by scientists, analysts, and engineers doing[scientific computing](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Scientific_computing)and technical computing.

SciPy contains modules for[optimization](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Optimization_(mathematics)),[linear algebra](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Linear_algebra),[integration](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Integral),[interpolation](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Interpolation),[special functions](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Special_functions),[FFT](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Fast_Fourier_transform),[signal](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Signal_processing)and[image processing](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Image_processing),[ODE](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Ordinary_differential_equation)solvers and other tasks common in science and engineering.

SciPy builds on the[NumPy](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/NumPy)array object and is part of the NumPy stack which includes tools like[Matplotlib](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Matplotlib),[pandas](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Pandas_(software))and[SymPy](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SymPy). There is an expanding set of scientific computing libraries that are being added to the NumPy stack everyday. This NumPy stack has similar users to other applications such as[MATLAB](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/MATLAB),[GNU Octave](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/GNU_Octave), and[Scilab](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Scilab). The NumPy stack is also sometimes referred to as the SciPy stack.[[2\]](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SciPy#cite_note-2)

SciPy is also a family of conferences for users and developers of these tools: SciPy (in the United States), EuroSciPy (in Europe) and SciPy.in (in India).[[3\]](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SciPy#cite_note-3)[Enthought](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Enthought)originated the SciPy conference in the United States and continues to sponsors many of the international conferences as well as host the[SciPy](https://link.jianshu.com/?t=http://www.scipy.org/)website.

The SciPy library is currently distributed under the[BSD license](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/BSD_license), and its development is sponsored and supported by an open community of developers. It is also supported by[Numfocus](https://link.jianshu.com/?t=http://www.numfocus.org/)which is a community foundation for supporting reproducible and accessible science.

Python Scientific Computing Environment[[edit](https://link.jianshu.com/?t=https://en.wikipedia.org/w/index.php?title=SciPy&action=edit&section=1)]

A typical Python Scientific Computing Environment includes many[dedicated software tools](https://link.jianshu.com/?t=http://scipy.org/topical-software.html). For example,

**Plotting**. The currently recommended 2-D plotting package is[Matplotlib](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Matplotlib), however, there are many other plotting packages such as[HippoDraw](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/HippoDraw),[Chaco](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Enthought), Biggles, and[Bokeh](https://link.jianshu.com/?t=http://bokeh.pydata.org/). Other popular graphics tools include[Python Imaging Library](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Python_Imaging_Library)and[MayaVi](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/MayaVi)(for 3D visualization).

**Optimization**. While SciPy has its own optimization package,[OpenOpt](https://link.jianshu.com/?t=https://en.wikipedia.org/w/index.php?title=OpenOpt&action=edit&redlink=1)has access to more optimization solvers and can involve[Automatic differentiation](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Automatic_differentiation).[CVXOpt](https://link.jianshu.com/?t=https://en.wikipedia.org/w/index.php?title=CVXOpt&action=edit&redlink=1)is another popular optimization library.

**Advanced data analysis**. Via RPy, Python can interface to the[R](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/R_(programming_language))statistical package for advanced data analysis.

**Database**. NumPy can interface with[PyTables](https://link.jianshu.com/?t=http://sourceforge.net/projects/pytables/), a hierarchical database package designed to efficiently manage large amounts of data using[HDF5](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/HDF5).

**Interactive shell**.[IPython](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/IPython)is an interactive environment that offers debugging and coding features similar to that which[MATLAB](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/MATLAB)offers.

**Symbolic mathematics**. There are several Python libraries—such as[PyDSTool](https://link.jianshu.com/?t=http://pydstool.sourceforge.net/)Symbolic and[SymPy](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SymPy)—that offer symbolic mathematics.

**Specialized extensions**. The["scikits"](https://link.jianshu.com/?t=http://scikits.appspot.com/)provide special-purpose add-ons to NumPy and Python. Of these,[scikit-image](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Scikit-image),[scikit-learn](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Scikit-learn)[[4\]](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SciPy#cite_note-4)and[statsmodels](https://link.jianshu.com/?t=https://en.wikipedia.org/w/index.php?title=Statsmodels&action=edit&redlink=1)are mature packages.

The SciPy Library/Package[[edit](https://link.jianshu.com/?t=https://en.wikipedia.org/w/index.php?title=SciPy&action=edit&section=2)]

The SciPy package of key algorithms and functions core to Python's scientific computing capabilities. Available sub-packages include:

**constants**: physical constants and conversion factors (since version 0.7.0[[5\]](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SciPy#cite_note-5))

**cluster**: hierarchical clustering, vector quantization, K-means

**fftpack**: Discrete Fourier Transform algorithms

**integrate**: numerical integration routines

**interpolate**: interpolation tools

**io**: data input and output

**lib**: Python wrappers to external libraries

**linalg**: linear algebra routines

**misc**: miscellaneous utilities (e.g. image reading/writing)

**ndimage**: various functions for multi-dimensional image processing

**optimize**: optimization algorithms including linear programming

**signal**: signal processing tools

**sparse**: sparse matrix and related algorithms

**spatial**: KD-trees, nearest neighbors, distance functions

**special**: special functions

**stats**: statistical functions

**weave**: tool for writing C/C++ code as Python multiline strings



[![img](https://upload.wikimedia.org/wikipedia/commons/thumb/5/54/Scipy_source.png/220px-Scipy_source.png)](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/File:Scipy_source.png)





Snapshot showing SciPy ndimage source code

Data structures[[edit](https://link.jianshu.com/?t=https://en.wikipedia.org/w/index.php?title=SciPy&action=edit&section=3)]

The basic data structure used by SciPy is a multidimensional array provided by the[NumPy](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/NumPy)module. NumPy provides some functions for linear algebra, Fourier transforms and random number generation, but not with the generality of the equivalent functions in SciPy. NumPy can also be used as an efficient multi-dimensional container of data with arbitrary data-types. This allows NumPy to seamlessly and speedily integrate with a wide variety of databases. Older versions of SciPy used Numeric as an array type, which is now deprecated in favor of the newer NumPy array code.[[6\]](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SciPy#cite_note-6)

History of SciPy[[edit](https://link.jianshu.com/?t=https://en.wikipedia.org/w/index.php?title=SciPy&action=edit&section=4)]

In the 1990s, Python was extended to include an array type for numerical computing called Numeric (This package was eventually replaced by Travis Oliphant who wrote NumPy in 2006 as a blending of Numeric and Numarray which had been started in 2001). In 1999, Travis Oliphant created a large collection of extension modules to enable scientific computing with Python and helped Pearu Peterson write f2py which enabled easily extending Python with Fortran code. This effort formed the foundation of SciPy. In 2001, Travis Oliphant and Pearu Peterson merged their efforts with a few modules that Eric Jones had written, and called the resulting package SciPy. The newly created package provided a standard collection of common numerical operations on top of the Numeric array data structure. Shortly thereafter, Fernando Pérez released IPython, an enhanced interactive shell widely used in the technical computing community, and John Hunter released the first version of Matplotlib, the 2D plotting library for technical computing. Since then the SciPy environment has continued to grow with more packages and tools for technical computing.[[7\]](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SciPy#cite_note-7)[[8\]](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SciPy#cite_note-8)[[9\]](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SciPy#cite_note-9)

See also[[edit](https://link.jianshu.com/?t=https://en.wikipedia.org/w/index.php?title=SciPy&action=edit&section=5)]



[![img](https://upload.wikimedia.org/wikipedia/commons/thumb/6/67/Nuvola_apps_emacs_vector.svg/28px-Nuvola_apps_emacs_vector.svg.png)](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/File:Nuvola_apps_emacs_vector.svg)

[Free software portal](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Portal:Free_software)



[List of numerical analysis software](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/List_of_numerical_analysis_software)

[Comparison of numerical analysis software](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Comparison_of_numerical_analysis_software)

[Sage (mathematics software)](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Sage_(mathematics_software))

External links[[edit](https://link.jianshu.com/?t=https://en.wikipedia.org/w/index.php?title=SciPy&action=edit&section=6)]

[SciPy website](https://link.jianshu.com/?t=http://www.scipy.org/)

[NumPy website](https://link.jianshu.com/?t=http://www.numpy.org/)

[SciPy Course Outline](https://link.jianshu.com/?t=http://www.rexx.com/~dkuhlman/scipy_course_01.html)by Dave Kuhlman

[Python Scientific Lecture Notes](https://link.jianshu.com/?t=http://scipy-lectures.github.com/)

Notes[[edit](https://link.jianshu.com/?t=https://en.wikipedia.org/w/index.php?title=SciPy&action=edit&section=7)]

**Jump up^**SciPy Team.["How can SciPy be fast if it is written in an interpreted language like Python?"](https://link.jianshu.com/?t=http://www.scipy.org/scipylib/faq.html#id12). Retrieved2013-12-23.

**Jump up^**["Scientific Computing Tools for Python"](https://link.jianshu.com/?t=http://www.scipy.org/about.html). SciPy.org.

**Jump up^**["SciPy Conferences"](https://link.jianshu.com/?t=http://conference.scipy.org/index.html).

**Jump up^**Bressert, Eli (2012).[*SciPy and NumPy: an overview for developers*](https://link.jianshu.com/?t=http://books.google.nl/books?id=fLKTuJqQLVEC&pg=PA43). O'Reilly. p. 43.

**Jump up^**["SciPy: Scientific Library for Python"](https://link.jianshu.com/?t=http://sourceforge.net/project/shownotes.php?release_id=660191&group_id=27747).[SourceForge](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/SourceForge).

**Jump up^**["NumPy Homepage"](https://link.jianshu.com/?t=http://www.numpy.org/).

**Jump up^**["History of SciPy"](https://link.jianshu.com/?t=http://wiki.scipy.org/History_of_SciPy).

**Jump up^**["Guide to NumPy"](https://link.jianshu.com/?t=http://csc.ucdavis.edu/~chaos/courses/nlp/Software/NumPyBook.pdf)(PDF).

**Jump up^**["Python for Scientists and Engineers"](https://link.jianshu.com/?t=http://www.computer.org/csdl/mags/cs/2011/02/mcs2011020009.html).