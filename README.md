Dlang UI
========
[![PayPayl donate button](https://img.shields.io/badge/paypal-donate-yellow.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=KPSNU8TYF6M5N "Donate once-off to this project using Paypal")

Cross platform GUI for D. Widgets, layouts, styles, themes, unicode, i18n, OpenGL based acceleration.

![screenshot](http://buggins.github.io/dlangui/screenshots/screenshot-example1-windows.png "Screenshot of widgets demo app example1")


GitHub page: [https://github.com/buggins/dlangui](https://github.com/buggins/dlangui)

Project site: [http://buggins.github.io/dlangui](http://buggins.github.io/dlangui)

API Documentation: [http://buggins.github.io/dlangui/ddox](http://buggins.github.io/dlangui/ddox)

Wiki: [https://github.com/buggins/dlangui/wiki/Home](https://github.com/buggins/dlangui/wiki/Home)

Getting Started Tutorial: [https://github.com/buggins/dlangui/wiki/Getting-Started](https://github.com/buggins/dlangui/wiki/Getting-Started)

Screenshots: [http://buggins.github.io/dlangui/screenshots.html](http://buggins.github.io/dlangui/screenshots.html)

Coding style: [https://github.com/buggins/dlangui/blob/master/CODING_STYLE.md](https://github.com/buggins/dlangui/blob/master/CODING_STYLE.md)

Main features:

* Crossplatform (Win32, OSX, Linux and Android are supported in current version)
* Mostly inspired by Android UI API (layouts, styles, two phase layout, ...)
* Supports highly customizable UI themes and styles
* Supports internationalization
* Hardware acceleration using OpenGL (when built with version USE_OPENGL)
* Fallback to pure Win32 API / SDL / X11 when OpenGL is not available (e.g. opengl dynamic library cannot be loaded)
* Actually it's a port (with major refactoring) of GUI library for cross platform OpenGL based implementation of Cool Reader app project from C++.
* Non thread safe - all UI operations should be preformed in single thread
* Simple 3d engine - allows to embed 3D scenes within GUI

D compiler versions supported
-----------------------------

Needs DMD frontend 2.100.2 or newer to build

Widgets
-------

* Widget - base class for all widgets and widget containers, similar to Android's View

Currently implemented widgets:

* TextWidget - simple static text (TODO: implement multiline formatting)
* ImageWidget - static image
* Button - simple button with text label
* ImageButton - image only button
* TextImageButton - button with icon and label
* CheckBox - check button with label
* RadioButton - radio button with label
* SwitchButton - a toggle switch button
* GroupBox - frame and caption for grouping other controls
* EditLine - single line edit
* EditBox - multiline editor
* VSpacer - vertical spacer - just an empty widget with layoutHeight == FILL_PARENT, to fill vertical space in layouts
* HSpacer - horizontal spacer - just an empty widget with layoutWidth == FILL_PARENT, to fill horizontal space in layouts
* ScrollBar - scroll bar
* SliderWidget - slider
* ProgressBarWidget - progress bar
* TabControl - tabs widget, allows to select one of tabs
* TabHost - container for pages controlled by TabControl
* TabWidget - combination of TabControl and TabHost
* GridWidgetBase - abstract Grid widget
* StringGrid - grid view with strings content
* TreeWidget - tree view
* ComboBox - combo box with text items
* ToolBar - tool bar with buttons
* StatusLine - control to show misc application statuses
* AppFrame - base class for easy implementation of apps with main menu, toolbars, status bar

Layouts
-------

Similar to layouts in Android

* LinearLayout - layout children horizontally or vertically depending on orientation
* VerticalLayout - just a LinearLayout with vertical orientation
* HorizontalLayout - just a LinearLayout with horizontal orientation
* FrameLayout - all children occupy the same place; usually only one of them is visible
* TableLayout - children are aligned into rows and columns of table
* ResizerWidget - put into LinearLayout between two other widgets to resize them using mouse
* ScrollWidget - allow to scroll its child if its dimensions are bigger than possible
* DockHost - layout with main widget and additional dockable panels around it
* DockWindow - dockable window with caption, usually to be used with DockHost

List Views
----------

Lists are implemented similar to Android UI API.

* ListWidget - layout dynamic items horizontally or vertically (one in row/column) with automatic scrollbar; can reuse widgets for similar items
* ListAdapter - interface to provide data and widgets for ListWidget
* WidgetListAdapter - simple implementation of ListAdapter interface - just a list of widgets (one per list item) to show

Resources
---------

Resources like fonts and images use reference counting. For proper resource freeing, always destroy widgets implicitly.

* FontManager: provides access to fonts
* Images: .png or .jpg images; if filename ends with .9.png, it's autodetected as nine-patch image (see Android drawables description)
* StateDrawables: .xml file can describe list of other drawables to choose based on widget's State (.xml files from android themes can be used directly)
* imageCache allows to cache unpacked images
* drawableCache provides access by resource id (string, usually filename w/o extension) to drawables located in specified list of resource directories.

Styles and Themes
-----------------

Styles and themes are a bit similar to ones in Android API.

* Theme is a container for styles. Can be load from XML theme resource file.
* Styles are accessible in theme by string ID.
* Styles can be nested to form hierarchy - when some attribute is missing in style, value from base style will be used.
* State substyles are supported: allow to change widget appearance dynamically based on its state.
* Widgets use style attributes directly from assigned style. When some attribute is being changed in widget, it creates its own copy of base style, 
which allows to modify some of attributes, while getting base style attributes if they are not changed in widget. This trick can minimize memory usage for widget attributes when 
standard values are used.
* Current default theme is similar to one in MS Visual Studio 2013
* Resources can be either embedded into executable or loaded from external resource directory in runtime

Important notice
================

If build of your app is failed due to dlangui or its dependencies, probably you have not upgraded dependencies.

Try following:
```sh
dub upgrade --force-remove
dub build --force
```
As well, sometimes removing of dub.json.selections can help.


Win32 builds
------------

* Under windows, uses SDL2 or Win32 API as backend.
* Optionally, may use OpenGL acceleration
* Uses Win32 API for font rendering.
* Optinally can use FreeType for font rendering.
* Executable size for release Win32 API based build is 830K.


Build and run demo app using DUB:
```sh
git clone --recursive https://github.com/buggins/dlangui.git
cd dlangui/examples/example1
dub run --build=release
```

To avoid showing console window add win_app.def file to your package source directory and add line to your dub.json.

win_app.def:
```json
"sourceFiles": ["$PACKAGE_DIR/src/win_app.def"]
```
dub.json:
```json
"sourceFiles-windows": ["$PACKAGE_DIR/src/win_app.def"],
```

Linux builds (DUB)
------------------

* Uses SDL2 as a backend.
* Uses FreeType for font rendering.
* Uses FontConfig to get list of available fonts.
* OpenGL can be optionally used for better drawing performance.

        libsdl2, libfreetype, libfontconfig

E.g. in Ubuntu, you can use following command to enable SDL2 backend builds:
```sh
sudo apt-get install libsdl2-dev
```
In runtime, .so for following libraries are being loaded (binary packages required):
```
freetype, opengl, fontconfig
```

Build and run on Linux using DUB:
```sh
cd examples/example1
dub run dlangui:example1
```
MacOS builds (DUB)
------------------
DlangUI theoretically supports MacOS, but I have no way of testing if it actually work.
The support is **not guaranteed**.

Other platforms
---------------

* Other platforms support may be added easy


Third party components used
---------------------------

* `binbc-opengl` - for OpenGL support
* `bindbc-freetype` + FreeType library support under linux and optionally under Windows.
* `bindbc-sdl` + SDL2 for cross platform support
* WindowsAPI bindings from http://www.dsource.org/projects/bindings/wiki/WindowsApi (patched)
* X11 binding when SDL2 is not used
* PNG and JPEG reading code is based on dlib sources


Hello World
--------------------------------------------------------------
```D
// myproject.d
import dlangui;
mixin APP_ENTRY_POINT;

/// entry point for dlangui based application
extern (C) int UIAppMain(string[] args) {
    // create window
    Window window = Platform.instance.createWindow("My Window", null);
    // create some widget to show in window
    window.mainWidget = (new Button()).text("Hello world"d).textColor(0xFF0000); // red text
    // show window
    window.show();
    // run message loop
    return Platform.instance.enterMessageLoop();
}
```

Sample dub.json:
--------------------------------
```json
{
    "name": "myproject",
    "description": "sample DLangUI project",

    "targetPath": "bin",
    "targetType": "executable",

    "dependencies": {
        "dlangui": "~master"
    }
}
```
    
Hello World using DML
--------------------------------------------------------------

DlangUI supports creation of widgets from markup.

DML - DlangUI Markup Language - similar to QML.

Example of complex UI easy created from text:
```D
module app;

import dlangui;

mixin APP_ENTRY_POINT;

/// entry point for dlangui based application
extern (C) int UIAppMain(string[] args) {
    // create window
    Window window = Platform.instance.createWindow("DlangUI example - HelloWorld", null);

    // create some widget to show in window
    //window.mainWidget = (new Button()).text("Hello, world!"d).margins(Rect(20,20,20,20));
    window.mainWidget = parseML(q{
        VerticalLayout {
            margins: 10
            padding: 10
            backgroundColor: "#C0E0E070" // semitransparent yellow background
            // red bold text with size = 150% of base style size and font face Arial
            TextWidget { text: "Hello World example for DlangUI"; textColor: "red"; fontSize: 150%; fontWeight: 800; fontFace: "Arial" }
            // arrange controls as form - table with two columns
            TableLayout {
                colCount: 2
                TextWidget { text: "param 1" }
                EditLine { id: edit1; text: "some text" }
                TextWidget { text: "param 2" }
                EditLine { id: edit2; text: "some text for param2" }
                TextWidget { text: "some radio buttons" }
                // arrange some radio buttons vertically
                VerticalLayout {
                    RadioButton { id: rb1; text: "Item 1" }
                    RadioButton { id: rb2; text: "Item 2" }
                    RadioButton { id: rb3; text: "Item 3" }
                }
                TextWidget { text: "and checkboxes" }
                // arrange some checkboxes horizontally
                HorizontalLayout {
                    CheckBox { id: cb1; text: "checkbox 1" }
                    CheckBox { id: cb2; text: "checkbox 2" }
                }
            }
            HorizontalLayout {
                Button { id: btnOk; text: "Ok" }
                Button { id: btnCancel; text: "Cancel" }
            }
        }
    });
    // you can access loaded items by id - e.g. to assign signal listeners
    auto edit1 = window.mainWidget.childById!EditLine("edit1");
    auto edit2 = window.mainWidget.childById!EditLine("edit2");
    // close window on Cancel button click
    window.mainWidget.childById!Button("btnCancel").click = delegate(Widget w) {
        window.close();
        return true;
    };
    // show message box with content of editors
    window.mainWidget.childById!Button("btnOk").click = delegate(Widget w) {
        window.showMessageBox(UIString("Ok button pressed"d), 
                UIString("Editors content\nEdit1: "d ~ edit1.text ~ "\nEdit2: "d ~ edit2.text));
        return true;
    };

    // show window
    window.show();

    // run message loop
    return Platform.instance.enterMessageLoop();
}
```
    

There is DMLEdit sample app in DlangUI/examples directory.

You can run it with dub:
```sh
dub run dlangui:dmledit
```
It allows to edit DML text and see how it will look like when loaded into app (F5 refreshes view).

Syntax highlight, bracket matching, go to error and other useful features are implemented.


DlangIDE project
------------------------------------------------------------

It is a project to build D language IDE using DlangUI library.

But it already can open DUB based projects, edit, build and run them.

Simple syntax highlight.

DCD integration: go to definition and autocompletion for D source code.

Project page: [https://github.com/buggins/dlangide](https://github.com/buggins/dlangide)

How to build and run using DUB:
```sh
git clone https://github.com/buggins/dlangide.git
cd dlangide
dub run
```
