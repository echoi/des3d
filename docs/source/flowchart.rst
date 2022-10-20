Usage
-----

.. _docker:

Using docker image
******************
TBA

.. _installation:

Manual installation
*******************

.. _requirements:

Requirements
============

1. A **C++ compiler** that supports C++11 standard. GNU g++ 5.0 or newer will suffice

2. **Boost::Program_options** library. Version 1.42 or newer. To install only this library, first download the source code from `https://www.boost.org <https://www.boost.org>`_. In the untarred source directory, run  

.. code-block:: console

   ./bootstrap.sh
  
In the same directory, build the library by running 

.. code-block:: console

   ./b2 --with-program_options -q
  
3. `MMG3D <https://www.mmgtools.org/mmg-remesher-downloads>`_

4. Python 3.2+

5. Numpy for post-processing

6. Optional: For importing a mesh in the *ExodusII* format, you need to install the library, ``exodus``, which is available as a part of SEACAS project `https://github.com/gsjaardema/seacas/ <https://github.com/gsjaardema/seacas/>`_


.. _configuration:

Configuration
=============

Modify ``BOOST_ROOT_DIR`` in Makefile if you manually built or installed boost library. If you followed the instructions above to build 
``Boost::Program_options`` library, set ``BOOST_ROOT_DIR`` to the untarred boost directory.

If you want to import an ``ExodusII`` mesh (.exo), set ``useexo = 1`` and ``ndims = 3``. Only 3D exodus mesh can be imported.
* Set ``EXO_INCLUDE`` and ``EXO_LIB_DIR`` paths.

.. _building:

Building
========

* Run ``make`` to build optimized executable.
* Or run ``make opt=0`` to build a debugging executable.
* Or run ``make openmp=0`` to build the executable without ``OpenMP``. This is necessary to debug the code under valgrind.

.. _running_des3d:

DES3D Source Code
*************

dynearthsol.cxx
========
* See Flowchart for summary of main function

.. image:: images\flowchart_main.png
   :width: 250


Running DES3D
*************

.. code-block:: console

   dynearthsol2d input.cfg
   
or

.. code-block:: console

   dynearthsol3d input.cfg
   
* Several example input files are provided under ``examples/`` directory. The
  format of the input file is described in ``examples/defaults.cfg``.
* Benchmark cases with analytical solution can be found under ``benchmarks/``
  directory.
* Execute the executable with ``-h`` flag to see the available input parameters
  and their descriptions.

Run-time warnings
=================
* While running, DES3D might print warnings on screen. An example is the warning about the potential race condition: e.g.,

.. code-block:: console

   ****************************************************************
   *    Warning: egroup-0 and egroup-2 might share common nodes.
   *             There is some risk of racing conditions.
   *             Please either increase the resolution or
   *             decrease the number OpenMP threads.
   ****************************************************************

* Please do pay attention and follow given suggestions if any.

.. _visualization:

Visualizing outputs
*******************

To convert the binary output files to VTU files, run

.. code-block:: console

   2vtk.py modelname

``modelname`` should be the one defined in the config file.

To see more usage information, i.e., producing .VTP files for marker data, run

.. code-block:: console

   2vtk.py -h

* Some of the simulation outputs can be disabled by editing ``2vtk.py`` and
  ``output.cxx``. A more convenient control will be provided in the future.
* The processed VTU (node and cell data) and VTP (marker data) files can be visualized with `Paraview <https://paraview.org>`_ or `Visit <https://visit-dav.github.io/visit-website/index.html>`_.
