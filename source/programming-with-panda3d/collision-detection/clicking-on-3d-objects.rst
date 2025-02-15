.. _clicking-on-3d-objects:

Clicking on 3D Objects
======================

The simplest way to click on 3D objects in Panda3D is to use very simplistic
collision detection coupled with event processing. First, after a
``CollisonTraverser`` and a
``CollisionHandler`` have been setup, attach
a ``CollisionRay`` node to the camera.
This node will have its ``setFromCollideMask()``
set to ``GeomNode.getDefaultCollideMask()`` in order to be
as general as possible. 

.. only:: python

    
    
    .. code-block:: python
    
        pickerNode = CollisionNode('mouseRay')
        pickerNP = camera.attachNewNode(pickerNode)
        pickerNode.setFromCollideMask(GeomNode.getDefaultCollideMask())
        pickerRay = CollisionRay()
        pickerNode.addSolid(pickerRay)
        myTraverser.addCollider(pickerNP, myHandler)
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        PT(MouseWatcher) mouseWatcher;
        PT(CollisionRay) pickerRay;
        CollisionTraverser myTraverser = CollisionTraverser("ctraverser");
        PT(CollisionHandlerQueue) myHandler;
        PT(CollisionNode) pickerNode;
        NodePath pickerNP;
        
        pickerNode = new CollisionNode("mouseRay");
        pickerNP = camera.attach_new_node (pickerNode);
        pickerNode->set_from_collide_mask(GeomNode::get_default_collide_mask());
        pickerRay = new CollisionRay();
        pickerNode->add_solid(pickerRay);
        myHandler = new CollisionHandlerQueue();
        myTraverser.add_collider(pickerNP, myHandler);
    
    


For any object that you want to be pickable you should add a flag to it. The
easiest way is to use the
``setTag()`` function:


.. only:: python

    
    
    .. code-block:: python
    
        object1.setTag('myObjectTag', '1')
        object2.setTag('myObjectTag', '2')
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        object1.set_tag("myObjectTag", "1");
        object2.set_tag("myObjectTag", "2");
    
    


The above example sets the tag
``'myObjectTag'`` on two objects in your
graph that you want to designate as pickable. We will check for the presence
of this tag after we get the response back from the collision system.


.. only:: python

    Because :ref:`Actors <loading-actors-and-animations>` uses a different setup
    the collision system will return the geometry but not the NodePath. Use
    ``object.setPythonTag('myObjectTag', 1)`` and
    ``object.getPythonTag('myObjectTag')`` instead to return the
    nodepath of an Actor.


Now assume that the function
``myFunction()`` is set up to be called
for the ``'mouse1'`` event. In
``myFunction()`` is where you call
``pickerRay.setFromLens(origin, destX, destY)``. This makes the ray's
origin ``origin`` and the ray's
vector the direction from
``origin`` to the point
(``destX``,
``destY``).


.. only:: python

    
    
    .. code-block:: python
    
        def myFunction():
           # First we check that the mouse is not outside the screen.
           if base.mouseWatcherNode.hasMouse():
              # This gives up the screen coordinates of the mouse.
              mpos = base.mouseWatcherNode.getMouse()
        
              # This makes the ray's origin the camera and makes the ray point 
              # to the screen coordinates of the mouse.
              pickerRay.setFromLens(base.camNode, mpos.getX(), mpos.getY())
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        void myFunction(){
           if (!mouseWatcher->has_mouse()){
              // The mouse is probably outside the screen.
              return;
           }
           // This gives up the screen coordinates of the mouse.
           LPoint2f mpos = mouseWatcher->get_mouse();
        
           // This makes the ray's origin the camera and makes the ray point 
           // to the screen coordinates of the mouse.
           pickerRay->set_from_lens(window->get_camera(0), mpos.get_x(), mpos.get_y());
        }
    
    


After this, you now call the traverser like any other collision, get the
closest object and "pick" it.

``C++ editor should add a ``\ \ `` version of the code below. ``



.. only:: python

    
    
    .. code-block:: python
    
        def myFunction():
           mpos = base.mouseWatcherNode.getMouse()
           pickerRay.setFromLens(base.camNode, mpos.getX(), mpos.getY())
        
           myTraverser.traverse(render)
           # Assume for simplicity's sake that myHandler is a CollisionHandlerQueue.
           if myHandler.getNumEntries() > 0:
              # This is so we get the closest object
              myHandler.sortEntries()
              pickedObj = myHandler.getEntry(0).getIntoNodePath()
    
    


The node returned by the collision system may not be the object itself, but
might be just a part of the object. In particular, it will be one of the
``GeomNodes`` that make up the
object. (The ``GeomNode`` class
contains the visible geometry primitives that are used to define renderable
objects in Panda3D.) Since your object might consist of more than one
``GeomNode``, what you probably
would prefer to get is the
``NodePath`` that represents the
parent of all of these ``GeomNodes``;
that is, the ``NodePath`` that you
set the ``'myObjectTag'`` tag on above.
You can use ``nodePath.findNetTag()`` to return
the parent ``NodePath`` that
contains a specified tag. (There are also other, similar methods on
``NodePath`` that can be used to
query the tag specified on a parent node, such as
``getNetTag()`` and
``hasNetTag()``. For simplicity, we
shall restrict this example to
``findNetTag()``.)

``C++ editor should remove the ``\ \ `` tags from the next line after adding a corresponding ``\ \ `` version of the code below it. ``



.. only:: python

    Now you can edit ``myFunction()`` to
    look like this:




.. only:: python

    
    
    .. code-block:: python
    
        def myFunction():
           mpos = base.mouseWatcherNode.getMouse()
           pickerRay.setFromLens(base.camNode, mpos.getX(), mpos.getY())
        
           myTraverser.traverse(render)
           # Assume for simplicity's sake that myHandler is a CollisionHandlerQueue.
           if myHandler.getNumEntries() > 0:
              # This is so we get the closest object.
              myHandler.sortEntries()
              pickedObj = myHandler.getEntry(0).getIntoNodePath()
              pickedObj = pickedObj.findNetTag('myObjectTag')
              if not pickedObj.isEmpty():
                 handlePickedObject(pickedObj)
    
    

