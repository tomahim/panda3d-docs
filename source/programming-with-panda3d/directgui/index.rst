.. _directgui:

DirectGUI
=========



.. only:: cpp

    Since DirectGUI is implemented in Python, it is useless to c++ programmers.
    However, there is another GUI system, PGui, that forms the foundation of
    DirectGUI and is in fact implemented in C++.
    
    Let's take a look at some PGui functions.
    
    The PGui C++ modules
    --------------------
    
    [PGButton]
    
    
    
    .. code-block:: cpp
    
        #include "PGButton.h"
    
    
    
    with PGButton you can create a freely definable clickable button.
    
    It can hold four different states:
    
    ready -> The default state of an untouched button
    
    pressed -> The pressed state (the user clicked on it)
    
    roll over -> When the user hovers over the button without clicking
    
    inactive -> Disabled state
    
    Example usage, creating a texture for each state:
    
    
    
    .. code-block:: cpp
    
        PT(PGButton) MyButton;
        MyButton = new PGButton("MyButton");
        MyButton->setup("Button",0.1);
        PT(Texture) ButtonReady=TexturePool::load_texture("button.png");
        PT(Texture) ButtonRollover=TexturePool::load_texture("button_active.png");
        PT(Texture) ButtonPressed=TexturePool::load_texture("button_pressed.png");
        PT(Texture) ButtonInactive=TexturePool::load_texture("button_inactive.png");
        
        // PGFrameStyle is a powerful way to change the appearance of the button:
        PGFrameStyle MyStyle=MyButton->get_frame_style(0); // frame_style(0): ready state
        MyStyle.set_type(PGFrameStyle::T_flat);
        
        MyStyle.set_texture(ButtonReady);    MyButton->set_frame_style(0,MyStyle);
        MyStyle.set_texture(ButtonRollover); MyButton->set_frame_style(1,MyStyle);
        MyStyle.set_texture(ButtonPressed);  MyButton->set_frame_style(2,MyStyle);
        MyStyle.set_texture(ButtonInactive); MyButton->set_frame_style(3,MyStyle);
        
        NodePath defbutNP = window->get_aspect_2d().attach_new_node(MyButton);
        defbutNP.set_scale(0.1);
        
        // Setup callback function
        framework.define_key(MyButton->get_click_event(MouseButton::one() ), "button press", &GUI_Callback_Button_Clicked, MyButton);
    
    
    
    An example of a callback function:
    
    
    .. code-block:: cpp
    
        static void GUI_Callback_Button_Clicked(const Event *ev, void *data)
        {
         PGButton* CurrentButton=(PGButton *)data;
         // Your action here
         printf("%s has been pressed.\n",CurrentButton->get_name().c_str());
        }
    
    [PGSliderBar]
    
    
    
    .. code-block:: cpp
    
        #include "PGSliderBar.h"
    
    A simple sliderbar.
    
    This slider basically has four components:
    
    The thumb button. This is the sliding part.
    
    The slider. This is the 'rail' the thumb button slides along.
    
    The left button. User can click on it to slide the thumb button to the left.
    
    The right button. User can click on it to slide the thumb button to the right.
    
    Using default initialisation, none of these components are textured.
    
    
    
    .. code-block:: cpp
    
        PT(PGSliderBar) Slider=new PGSliderBar("MySliderBar");
         
        // Setup, feeding the constructor with (bool vertical,float lenght,float width,float bevel)
        Slider->setup_slider(false,0.1,0.1,0); // 'rail' properties
        Slider->set_range(0,1);
        Slider->set_value(0.5);
        
        // Setup scroll bar (the 'moving thumb button' including left and right button)
        Slider->setup_scroll_bar(false,0.35,0.05,false);
        NodePath SliderNP=window->get_aspect_2d().attach_new_node(Slider);
        SliderNP.set_pos(0,0,0);
    
    
    
    (work in progress, more to come soon.)




.. only:: python

    The DirectGUI Python modules
    ----------------------------
    
    Panda3D comes with a set of tools for the creation of a graphical interface
    for any program. The DirectGui system is used to create buttons, labels, text
    entries, and frames within the program. All of these items can be decorated
    with text, images, and 3D graphics. Commands may be associated with these
    items as well. Since these objects inherit from the NodePath class, anything
    done to a NodePath may be done to them, such as show()/hide(), setPos(),
    posInterval(), and so on. Also, since DirectGui objects are by default
    parented to the node aspect2d, they will stay on the screen no matter how the
    user navigates through the world.
    
    You can specify the parent nodepath for any DirectGUI object using the parent=
    argument. You can use base.aspect2d for center-oriented pacement, and
    a2dTopLeft or a2dBottomRight respectively. The Y coordinate should be left 0,
    because it's useless in the 2D space. The Z coordinate goes from -1 to 1, and
    X depends on the aspect ratio, by the default 4:3 it's from -1.3333 to 1.3333.
    The other aspect2d variants don't scale the numbers, just offset them.
    
    The direct-gui-edit option in the Config.prc file allows the user to use the
    middle mouse button to move around widgets, and resize them while holding the
    control key; this is very useful to lay a screen out during development. If
    you need to turn this ability off for an individual object, set its
    ``enableEdit`` keyword parameter to
    False.
    
    All of the DirectGui objects are constructed in a similar way:
    
    
    
    .. code-block:: python
    
        from direct.gui.DirectGui import *
        myObject = Directxxxxxx(keyword=value, keyword=value, ...)
    
    
    
    Each DirectGui object may contain any of four fundamental pieces that
    determine its appearance. There may be an optional text, an optional geom, an
    optional image, and an optional frame.
    
    A DirectGui's text label may be any arbitrary text string, and whatever text
    string you supply is automatically created using the :ref:`onscreentext`
    interface and centered on the object. You can specify the text string using
    the ``text`` keyword. You can
    also specify further parameters to control the appearance or placement of the
    text using the form ``text_parameter``,
    where ``parameter`` is any valid
    keyword to the :ref:`onscreentext` constructor.
    
    A DirectGui's geom can be any NodePath that you design, to represent the
    appearance of the gui object. Typically, this will be a model that you created
    via the program ``egg-texture-cards``. This
    little program takes a png or other picture and turns it into an egg. See
    :ref:`Automatic_Texture_Animation <automatic-texture-animation>`, for more
    details. Using this interface, you can completely customize the look of the
    DirectGui object to suit your needs. You can specify the geom object using the
    ``geom`` keyword, and like the
    text parameter, you can also control the geom's placement using keywords like
    ``geom_parameter``.
    
    The image is less often used. It is the filename of a texture image (or an
    already-loaded Texture object). It is intended for displaying a simple texture
    image for which you don't already have a model created via
    ``egg-texture-cards``. A default card will be
    created to display this texture, with a bounding box of (-1, 0, -1) to (1, 0,
    1); that is, a square with sides of length 2 units, centered on the origin.
    You can position and scale this card with the keywords
    ``image_pos`` and
    ``image_scale``. See also
    :ref:`onscreenimage`
    
    Finally, the DirectGui may have a frame created for it. This is typically a
    gray rectangular background with an optional bevel. There are a handful of
    different frame styles; you can use the
    ``relief`` keyword to select from
    one of the available styles; your choices are SUNKEN, RAISED, GROOVE, or
    RIDGE. You can also specify
    ``relief = None`` to avoid creating a
    frame polygon altogether (this is commonly done when you have specified your
    own geom object with the
    ``geom`` keyword).
    
    The overall size of the DirectGui object is controlled with the
    ``frameSize`` keyword. This is a
    four-tuple of floating-point numbers of the form (left, right, bottom, top),
    which specifies the bounding box region of the DirectGui object. That is, the
    lower-left corner will be at position (left, 0, bottom), and the upper-right
    will be at (right, 0, top). Note that these values represent coordinates from
    the origin of the frame. Setting the frameSize to (-0.1, 0.1, -0.1, 0.1), for
    instance, will create a box, 0.2 units wide and 0.2 units in height, with 0,0
    being the center of the frame located at
    ``pos`` on the screen.
    
    The ``frameSize`` keyword is
    optional. If you omit it, the default frameSize is computed based on the
    bounding box of the text, geom, and/or image that you have specified.
    
    The following is a list of keywords that are typically available to DirectGui
    objects of all kinds. Individual kinds of DirectGUI objects may add more
    options to this list, but these keywords are not repeated on each of the
    following pages, for brevity:
    
    ================= ================================================================================================================================================================================================================ =========================================================
    Keyword           Definition                                                                                                                                                                                                       Value
    ================= ================================================================================================================================================================================================================ =========================================================
    text              Text to be displayed on the object                                                                                                                                                                               String
    text_bg           Background color of the text on the object                                                                                                                                                                       (R,G,B,A)
    text_fg           Color of the text                                                                                                                                                                                                (R,G,B,A)
    text_pos          Position of the displayed text                                                                                                                                                                                   (x,z)
    text_roll         Rotation of the displayed text                                                                                                                                                                                   Number
    text_scale        Scale of the displayed text                                                                                                                                                                                      (sx,sz)
    text_\*           Parameters to control the appearance of the text                                                                                                                                                                 Any keyword parameter appropriate to :ref:`onscreentext`.
    frameSize         Size of the object                                                                                                                                                                                               (Left,Right,Bottom,Top)
    frameVisibleScale Relative scale of the visible frame to its clickable bounds. Useful for creating things like the paging region of a slider, which is visibly smaller than the acceptable click region (the height of the thumb). (hscale, vscale)
    frameColor        Color of the object’s **frame**                                                                                                                                                                                  (R,G,B,A)
    relief            Relief appearance of the **frame**                                                                                                                                                                               SUNKEN, RAISED, GROOVE, RIDGE, FLAT, or None
    invertedFrames    If true, switches the meaning of SUNKEN and RAISED                                                                                                                                                               0 or 1
    borderWidth       If relief is SUNKEN, RAISED, GROOVE, or RIDGE, changes the size of the bevel                                                                                                                                     (Width,Height)
    image             An **image** to be displayed on the object                                                                                                                                                                       image filename or Texture object
    image_pos         Position of the displayed image                                                                                                                                                                                  (x,y,z)
    image_hpr         Rotation of the displayed image                                                                                                                                                                                  (h,p,r)
    image_scale       Scale of the displayed image                                                                                                                                                                                     (sx,sy,sz)
    geom              A **geom** to represent the object’s appearance                                                                                                                                                                  NodePath
    geom_pos          Position of the displayed geom                                                                                                                                                                                   (x,y,z)
    geom_hpr          Rotation of the displayed geom                                                                                                                                                                                   (h,p,r)
    geom_scale        Scale of the displayed geom                                                                                                                                                                                      (sx,sy,sz)
    parent            Parent to attach to                                                                                                                                                                                              NodePath
    pos               Position of the object                                                                                                                                                                                           (X,Y,Z)
    hpr               Orientation of the object                                                                                                                                                                                        (H,P,R)
    scale             Scale of the object                                                                                                                                                                                              Number
    pad               When frameSize is omitted, this determines the extra space around the **geom** or **text**\ ’s bounding box by which to expand the default **frame**                                                             (Width,Height)
    state             The initial state of the object                                                                                                                                                                                  NORMAL or DISABLED
    frameTexture      Texture applied directly to the generated **frame**                                                                                                                                                              image filename or Texture object
    enableEdit        Affects direct-gui-edit functionality                                                                                                                                                                            0 or 1
    suppressKeys      If 1, suppresses triggers of global keyboard-related Panda events (not part of the GUI system)                                                                                                                   0 or 1
    suppressMouse     If 1, suppresses triggers of global mouse-related Panda events (e.g. camera controls)                                                                                                                            0 or 1
    sortOrder         Specifies render order for overlapping objects. Higher numbers are drawn in front of lower numbers.                                                                                                              Number
    textMayChange     Whether the text of an object can be changed after creation                                                                                                                                                      0 or 1
    ================= ================================================================================================================================================================================================================ =========================================================
    
    Remember that the axes for Panda3D use x for left and right, y for in and out
    of the screen, and z for up and down. An object's frame is always in the
    background of the object. The geom, if any, is shown in front of the frame,
    and text is shown in front of the geom.
    
    It is possible to change most of these values after object creation, using:
    
    
    
    .. code-block:: python
    
        myDirectObject['keyword'] = value
    
    
    
    Most properties can be updated in this way, although position and other
    transform-related values cannot be updated via the keyword
    parameters--attempts to update them will silently fail. Instead, use the
    NodePath methods to change the object's transform.
    
    Some types of updates, such as changing the text or the geom, may also change
    the size of the object. If you change any of these properties after the object
    has been created, it is necessary to tell the object to re-determine its size:
    
    
    
    .. code-block:: python
    
        myDirectObject.resetFrameSize()
    
    
    
    If you don't do this, you may find, for example, that a button isn't clickable
    because it believes it has a zero-width frame.
    
    To permanently remove a DirectGUI object, you should use the method:
    
    
    
    .. code-block:: python
    
        myDirectObject.destroy()
    
    
    
    It is not sufficient to simply call
    ``removeNode()``, since the DirectGUI
    system adds a number of messenger hooks that need to be cleaned up. However,
    if you have a hierarchy of DirectGUI objects, for instance a number of buttons
    parented to a frame, it is sufficient to call destroy() only on the topmost
    object; it will propagate downwards.



.. toctree::
   :maxdepth: 2

   directbutton
   directcheckbutton
   directradiobutton
   directdialog
   directentry
   directframe
   directlabel
   directoptionmenu
   directscrolledlist
   directwaitbar
   directslider
   directscrollbar
   directscrolledframe
