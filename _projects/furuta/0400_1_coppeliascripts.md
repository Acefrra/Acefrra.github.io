---
    project: furuta
    type: subsect
    section: Building Simulation Scene
    title: Coppelia Scripts
---

The strength of Coppelia is the possibility of controlling the scene using lua-based scripts. Every coppelia object can be attqached to some child scripts. Those can be:

* **Non-threaded child script**, they contain a collection of blocking functions. This means that every time they are called, they should perform some task and then return control. If control is not returned, then the whole simulation halts. These functions are
    * **sysCall_init()**, which is executed just one time when simulation starts
    * **sysCall actuation()**, it loops code over time, generally to actuate joints of the system;
    * **sysCall sensing()**, same as actuation, but it's for reading (computing) some relevant variable of the system
* **Threaded child script**, these are scripts that will launch in a thread. The launch (and resume) of threaded child scripts is handled by the default main script code. They won't be used in this project.

The Non-threaded script will be used mainly to perform the electromechanical actuation starting from the control voltage input fed by Matlab.

Then a **Customization script** has been defined in order to measure the whole state of the system. They are different from child script as they contain a set of function that will be called either from a child script or from an external process.

In Coppelia each object has an **handle** which identifies univocally that object. Through the handle and the built-in Coppelia functions it is possible to access bodies and joints properties and modify them.





