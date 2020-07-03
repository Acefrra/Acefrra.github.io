---
    layout: documentation

    project: furuta
    title: Building Simulation Scene
    type: sect
---

The CoppeliaSim simulation scene has been created:

{% include_relative /images/Scene.html %}

The system is made up of 3 bodies:

* Frame, which holds the Furuta Pendulum and keep it lifted from ground;
* Rotating Arm;
* Pendulum;

We don't want the Frame to move, for this reason the body will not be a **dynamical** body in the scene. On the other hand, Rotating Arm and Pendulum will be. Their mass will be set respectively to 0.125kg and 0.250kg. As for the moment of inertia, they are to be set for three axes. They will be put to 0.0023 kg m^2. Moreover since we don't want the arm and the pendulum to collide each other we will set the respondable mask accordingly.

{% include_relative /images/Frame.html %}

{% include_relative /images/Pend_Rot_Properties.html %}



