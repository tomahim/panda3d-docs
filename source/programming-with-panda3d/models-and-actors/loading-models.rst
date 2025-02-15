.. _loading-models:

Loading Models
==============

\__NOTOC_\_

The Basics
----------




.. only:: python

    Loading static geometry is done using
    ``loader.loadModel``:
    
    
    
    .. code-block:: python
    
        m = loader.loadModel("mymodel.egg")
    
    



.. only:: cpp

    Loading static geometry is done using
    ``window->load_model``:
    
    
    
    .. code-block:: cpp
    
        NodePath m = window->load_model(framework.get_models(), "mymodel.egg");
    
    


The path name specified in the loadModel can be an absolute path, or a
relative path. Relative is recommended. If a relative path is used, then
Panda3D will search its model path to find the egg file. The model path is
controlled by panda's :ref:`configuration file <the-configuration-file>`.

Inserting the Model into the Scene Graph
----------------------------------------


Do not forget that loading the model does not, by itself, cause the model to
be visible. To cause Panda3D to render the model, you must insert it into the
scene graph:



.. only:: python

    
    
    .. code-block:: python
    
        m.reparentTo(render)
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        m.reparent_to(window->get_render());
    
    


You can read more about :ref:`the-scene-graph`.

Panda Filename Syntax
---------------------


The path used in the 

.. only:: python

    loadModel



.. only:: cpp

    load_model


``call must abide by Panda3D's filename``

conventions. For easier portability, Panda3D uses Unix-style pathnames, even
on Microsoft Windows. This means that the directory separator character is
always a forward slash, not the Windows backslash character, and there is no
leading drive letter prefix. (Instead of a leading drive letter, Panda uses an
initial one-letter directory name to represent the drive.)

There is a fairly straightforward conversion from Windows filenames to panda
filenames. Always be sure to use Panda filename syntax when using a Panda3D
library function, or one of the panda utility programs:



.. only:: python

    
    
    .. code-block:: python
    
        # WRONG:
        loader.loadModel("c:\\Program Files\\My Game\\Models\\Model1.egg")
        
        # RIGHT:
        loader.loadModel("/c/Program Files/My Game/Models/Model1.egg")
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        # WRONG:
        window->load_model(framework.get_models(), "c:\\Program Files\\My Game\\Models\\Model1.egg");
        
        # RIGHT:
        window->load_model(framework.get_models(), "/c/Program Files/My Game/Models/Model1.egg");
    
    


Panda uses the ``Filename`` class to
store Panda-style filenames; many Panda functions expect a Filename object as
a parameter. The Filename class also contains several useful methods for path
manipulation and file access, as well as for converting between Windows-style
filenames and Panda-style filenames; see the API reference for a more complete
list.

To convert a Windows filename to a Panda pathname, use code similar to the
following:



.. only:: python

    
    
    .. code-block:: python
    
        from panda3d.core import Filename
        winfile = "c:\\MyGame\\Model1.egg"
        pandafile = Filename.fromOsSpecific(winfile)
        print pandafile
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        #include "filename.h"
        
        const string winfile = "c:\\MyGame\\Model1.egg";
        Filename pandafile = Filename::from_os_specific(winfile);
        std::cout << pandafile.get_fullpath() << "\n";
    
    


To convert a Panda filename into a Windows filename, use code not unlike this:



.. only:: python

    
    
    .. code-block:: python
    
        from panda3d.core import Filename
        pandafile = Filename("/c/MyGame/Model1.egg")
        winfile = pandafile.toOsSpecific()
        print winfile
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        #include "filename.h"
        
        Filename pandafile ("/c/MyGame/Model1.egg");
        const string winfile = pandafile.to_os_specific();
        std::cout << winfile << "\n";
    
    


The Filename class can also be used in combination with


.. only:: python

    python's built-in path manipulation mechanisms



.. only:: cpp

    Panda's ExecutionEnvironment utility class
. Let's say, for
instance, that you want to load a model, and the model is in the "model"
directory that is in the same directory as the main program's


.. only:: python

    "py"



.. only:: cpp

    executable


``file.  Here is how you``

would load the model:



.. only:: python

    
    
    .. code-block:: python
    
        import sys,os
        import direct.directbase.DirectStart
        from panda3d.core import Filename
        
        # Get the location of the 'py' file I'm running:
        mydir = os.path.abspath(sys.path[0])
        
        # Convert that to panda's unix-style notation.
        mydir = Filename.fromOsSpecific(mydir).getFullpath()
        
        # Now load the model:
        model = loader.loadModel(mydir + "/models/mymodel.egg")
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        #include "filename.h"
        #include "executionEnvironment.h"
        
        // Get the location of the executable file I'm running:
        Filename mydir = ExecutionEnvironment::get_binary_name();
        mydir = mydir.get_dirname();
        
        // Now load the model:
        window->load_model(framework.get_models(), mydir + "/models/mymodel.egg");
    
    


You need to keep in mind that the


.. only:: cpp

    Operating System's


| ``standard 

.. only:: python

    python
``
| ``functions 

.. only:: python

    (like os.remove())
``
| ``work with OS specific paths. So do not forget to convert your OS Generic paths back to OS Specific paths when using built-in functions. In cases that Panda offers equivalent functions through the Filename class, it is recommended to use that instead.``
