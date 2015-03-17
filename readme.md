# SBNW: A Network Viewer for SBML

## Introduction
SBNW is a network viewer which supports autolayout of models. Uses libSBML for reading/writing models.

## Download

The latest releases can be downloaded at https://github.com/0u812/sbnw/releases.

## SBNW: An Automatic Layout Tool for SBML

Online C API documentation (via Doxygen) can be found at http://0u812.github.io/sbnw/docs. Python documentation can be found at http://0u812.github.io/sbnw/sphinx.


## How to compile the library

 * Install the latest version of <a href="http://sourceforge.net/projects/sbml/files/libsbml/">libSBML</a> (tested with 5.6, 5.8, 5.10, 5.11)
 * Clone the latest revision of the <a href="https://github.com/0u812/sbnw">master branch</a> via git
 * Download and run <a href="http://www.cmake.org/">CMake</a> (compatible with major version 2 or 3)
 * Using CMake, set the `LIBSBML_DIR` variable to point to the directory where libSBML is installed/downloaded
 * Configure & generate via CMake, choosing a generator that matches the required configuration (32-bit x86 is recommended on Windows; on Linux the default generator is sufficient)
 * Open the generated .sln in Visual Studio, and change the configuration to "Release" (the easy step)
 * (This step was previously used to instruct the user to set the MSVC runtime library. It is now set automatically through CMake. This placeholder serves as a reminder in case this solution breaks at some point)
 * Shift-B will build all targets
 * If the build is successful, right click on the INSTALL target and select build. SBNW will be installed to the location stored in CMAKE_INSTALL_PREFIX (ensure your user has write access)

## Using the Library (C API)

The C API is exposed via graphfab/autolayoutc_api.h.  There's an example in sandbox/basic that demonstrates all the functionality you need to get the layout working.  The relevant lines are:

```
        // type to store layout info
        gf_layoutInfo* l;

        // load the model
        gf_SBMLModel* mod = gf_loadSBMLbuf(buf);

        // options for layout algo
        fr_options opt;

        // read layout info from the model
        l = gf_processLayout(mod);

        // randomize node positions
        gf_randomizeLayout(l);

        // do layout algo
        opt.k = 20.;
        opt.boundary = 1;
        opt.mag = 0;
        opt.grav = 0.;
        opt.baryx = opt.baryy = 500.;
        opt.autobary = 1;
        gf_doLayoutAlgorithm(opt, l);

        // save layout information to new SBML file
        gf_writeSBMLwithLayout(outfile, mod, l);

        // run destructors on the model
        gf_freeSBMLModel(mod);

        // run destructors on the layout
        gf_freeLayoutInfo(l);
```