arduino-plotter
===============
An Arduino library for easy graphing on host computer via serial communication

_by: Devin Conley_

Features:
---
- Continuous multi-variable plots against time
- 2-variable "x" vs "y" plots
- Display multiple graphs within single resizable window
- Support for any data type that can be cast to a double
- Simply pass a reference to your variables when the graph is added, no need to update each value explicitly
- Control number of data points displayed on each graph
- Auto-scaling to fit all data on graph
- Configurable line color per variable
- Stand-alone listener application, written with Processing, is provided

![Plotter Preview](https://www.dropbox.com/s/2mtg5ig7lyrrffi/plotter_preview.gif?raw=1)

---

Quickstart:
---

#### Install Plotter library 
Search for "Plotter" in the Arduino Library Manager.

___or___

Install manually with the [ZIP file of Plotter](https://github.com/devinaconley/arduino-plotter-for-library-manager/archive/master.zip).

---

#### Setup Listener
Download one of the following stand-alone listener options. Keep the folder intact so the application can access the library and source folders. **(Java Runtime Environment 8 required)**
- [Windows 32-bit](https://www.dropbox.com/s/vbgd4osedrfgep3/arduino-plotter-listener-windows32.zip?dl=1)
- [Windows 64-bit](https://www.dropbox.com/s/0x4xqv375h4j5hh/arduino-plotter-listener-windows64.zip?dl=1)
- [Linux 32-bit](https://www.dropbox.com/s/u0g1ey96aw14k9z/arduino-plotter-listener-linux32.zip?dl=1)
- [Linux 64-bit](https://www.dropbox.com/s/lsrpo5edb25sv3a/arduino-plotter-listener-linux64.zip?dl=1)
- [Mac OS X](https://www.dropbox.com/s/k6ej5jftofotlhk/arduino-plotter-listener-macosx.zip?dl=1)

___or___

Download the source directory from this repository (listener/) and run that with the Processing IDE.

---

#### Usage in Arduino code
Include library
```arduino
#include "Plotter.h"
```

Declare global variables. Any variable that you want plotted should have global scope. You will also likely want the Plotter object to be accessible globally.
```arduino
double x;

Plotter p;
```

Create the Plotter object and add graphs as desired. When adding graphs, the first argument is a String with the title of the graph and the second argument is an int with the number of points displayed at any given time. These two arguments are followed by a String and your corresponding variable.
```arduino
void setup()
{
  p = Plotter();
  
  p.AddTimeGraph( "Some title of a graph", 500, "label for x", x );
}
```

Update variables as you normally would and call plot whenever you are ready. This example simply assigns arbitrary sine data.
```arduino
void loop()
{
  x = 10*sin( 2.0*PI*( millis() / 5000.0 ) );

  p.Plot(); // usually called within loop()
}
```

---

#### Using the Listener
Once the Arduino is running, start the listener application that you setup above.

![Start Listener Image](https://www.dropbox.com/s/9kyzory64369mjh/start_listener.png?raw=1)

The application will configure itself and your data should be plotted appropriately.

![Quick Start Results Image](https://www.dropbox.com/s/jcj7wilsu8fbzia/quickstart.png?raw=1)

---

Documentation:
---

#### void AddTimeGraph( String title, int pointsDisplayed, String label1, Variable1Type variable1 )

*Add a 1-variable graph vs. time*
##### Arguments
- title: String with title of graph
- pointsDisplayed: number of points to be shown at a given time. Used to control time-scaling
- label1: String with label of the plotted variable
- variable1: global variable that will be updated throughout program

---

#### void AddTimeGraph( String title, int pointsDisplayed, String label1, Variable1Type variable1, String label2, Variable2Type variable2, ... )

*Add a multi-variable graph vs. time*
##### Arguments
- title: String with title of graph
- pointsDisplayed: number of points to be shown at a given time. Used to control time-scaling
- label1: String with label of the plotted variable
- variable1: global variable that will be updated throughout program
- label2: String with label of the plotted variable
- variable2: global variable that will be updated throughout program
- ...

---

#### void AddXYGraph( String title, int pointsDisplayed, String labelX, VariableTypeX variableX, String labelY, VariableTypeY variableY )

*Add an X vs Y graph*
##### Arguments
- title: String with title of graph
- pointsDisplayed: number of points to be shown at a given time. Used to control time-scaling
- labelX: String with label of variable to be plotted along X-axis
- variableX: global X-variable that will be updated throughout program
- labelY: String with label of variable to be plotted along Y-axis
- variableY: global Y-variable that will be updated throughout program

---

#### void Plot()

*Send most recent values for all plotted variables to the listener application*

---

#### bool Remove( int index )

*Removes the graph at the specified position with zero-indexing. (ie passing 0 would remove the first graph added)*
##### Arguments
- index: position of graph to remove

##### Returns
- bool: true if successful

---

#### bool SetColor( int index, String colorA, String colorB, ... )

*Sets the line colors for each variable in the graph at the specified position with zero-indexing. (ie passing 0 would set colors for the first graph added)*

*Options: "red", "green", "blue", "orange", "yellow", "pink", "purple", "cyan"*
##### Arguments
- index: position of graph to remove
- colorA: color for first variable
- colorB: color for second variable
- ...

##### Returns
- bool: true if successful. returns false when incorrect number of colors have been passed.
