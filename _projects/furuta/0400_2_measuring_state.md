---
    project: furuta
    type: subsect
    section: Building Simulation Scene
    title: Measuring State
---

Before applying any feedback from Matlab it is first necessary to compute thet state

$$ \begin{bmatrix} \theta(t) & \alpha(t) & \dot \theta(t) & \dot \alpha(t) \end{bmatrix} $$.

Coppelia provides functions to compute the joint coordinate and velocity. That is $$ q_{i} $$ and $$ \dot q_{i} $$ of the joint. However For the joint position, some numerical error have been showed up when implementing the controller. Another approach has been applied which was to compute them manually using the transformation matrixes.

For the body **Rotating Arm** the transformation matrix $$ M_{0}^{1}$$ from Frame to Rotating Arm has been gotten. It is a 4x4 matrix where at the last column there is the position of the origin of the reference of the arm with respect to ground. By using simple trigonometric relations it has been possible to compute the respective angle $$\theta$$. For the **Pendulum** we computed $$M_{1}^{2}$$ which is the transformation matrix from the Rotating Arm to the Pendulum to get the angle $$\alpha$$.

Since the $$atan2(y,x)$$ function has been used, the angle will vary within $$[-\pi, \pi]$$. In particular, this function shows a discontinuity when the angle jump from $$-\pi$$ to $$\pi$$ when passing from the configuration with $$ x < 0 $$ and $$y-> 0^{-}$$ to the one with $$ y->0^{+} $$ and viceversa.

For the angle theta we don't want this discontinuity. Infact if the reference is for example $$-3.10\text{ rad.}$$ then the pendulum may go past the discontinuity and this would produce a large error so that the control input becomes too large and the system diverge. For this reason, for $$\theta$$ we want to extend the image to the whole $$\R$$ and avoid wrapping of the angle. A counter to count the number of revolutions has been used to give more information about the system configuration. In particular, the piece of code accomplishing this is: 

<pre>
    <code>
        actual_alpha = math.atan2(arm2pendulum[4], arm2pendulum[8]);
        --Computing alphadot -- For alpha there is no counter otherwise it screws the swing-up control
        alphadot = (actual_alpha - previous_alpha)/(actual_time-previous_time);
        if(actual_alpha*previous_alpha<0) then
            if (math.abs(previous_alpha)>1) then
                alphadot_abs = (math.abs((math.pi-math.abs(actual_alpha)) + (math.pi -math.abs(previous_alpha))))/(actual_time-previous_time);
                if actual_alpha>0 then
                    alphadot = alphadot_abs;
                else
                    alphadot = -alphadot_abs;
                end
            end
        end
        alpha_correct = actual_alpha; --We do not correct the angle alpha
    </code>
</pre>

In this way the angle theta will vary between $$\pm \infty$$.
For the angle alpha instead we do not want an unlimited range. This is because when the pendulum is upward we always want the angle alpha to be 0 and not $$\pm 2 \pi k$$. In fact we do not want the swing up control to work just on "one side of rotation". On the hand we want to make sure that the derivative will not jump when crossing the disconituinty. This is the realtive code:

<pre>
    <code>
        actual_theta = -math.atan2(frame2arm[4], frame2arm[8]);
        --Updating counter for theta
        if(actual_theta*previous_theta<0) then
            if (math.abs(previous_theta)>1) then
                if actual_theta>0 then
                    c_theta = c_theta - 1;
                else
                    c_theta = c_theta + 1;
                end
            end
        end
        --Correcting theta and computing its derivative
        theta_correct = actual_theta + c_theta*2*math.pi;
        thetadot = (theta_correct - previous_theta_correct)/(actual_time-previous_time);
    </code>
</pre>

All in all the corresponding Customization script code will be:
<pre>
    <code>
    -- See the user manual or the available code snippets for additional callback functions and details
    -- This is the sensing code for the state of the system. This set of function will be called by Matlab
    -- The sensing code in the child script @Frame just compute thetadot for the electro actuation
    function init_measure_state()
        Pendulum = sim.getObjectHandle("Pendulum");
        RotatingArm = sim.getObjectHandle("RotatingArm");
        Frame = sim.getObjectHandle("Frame");
        --Getting the Transformation Matrix to measure the components of the CM of Pendulum and RotatingArm w.r.t. previous body
        arm2pendulum = sim.getObjectMatrix(Pendulum, RotatingArm);
        frame2arm = sim.getObjectMatrix(RotatingArm,Frame);
        previous_time = sim.getSimulationTime()-0.01;
        --Compute the angle starting from the (y,z) components of the Center of Mass
        previous_theta = -math.atan2(frame2arm[4], frame2arm[8])-0.01;
        previous_theta_correct = previous_theta;
        previous_alpha = math.atan2(arm2pendulum[4], arm2pendulum[8])-0.01;
        --Initializing counter theta for correcting the corresponding angle
        c_theta = 0;
    end

    function measure_state ()
        --Getting TM
        arm2pendulum = sim.getObjectMatrix(Pendulum, RotatingArm);
        frame2arm = sim.getObjectMatrix(RotatingArm,Frame);
        --Time
        actual_time = sim.getSimulationTime();
        --Computing angles
        actual_alpha = math.atan2(arm2pendulum[4], arm2pendulum[8]);
        actual_theta = -math.atan2(frame2arm[4], frame2arm[8]);
        --Updating counter for theta
        if(actual_theta*previous_theta<0) then
            if (math.abs(previous_theta)>1) then
                if actual_theta>0 then
                    c_theta = c_theta - 1;
                else
                    c_theta = c_theta + 1;
                end
            end
        end
        --Computing alphadot -- For alpha there is no counter otherwise it screws the swing-up control
        alphadot = (actual_alpha - previous_alpha)/(actual_time-previous_time);
        if(actual_alpha*previous_alpha<0) then
            if (math.abs(previous_alpha)>1) then
                alphadot_abs = (math.abs((math.pi-math.abs(actual_alpha)) + (math.pi -math.abs(previous_alpha))))/(actual_time-previous_time);
                if actual_alpha>0 then
                    alphadot = alphadot_abs;
                else
                    alphadot = -alphadot_abs;
                end
            end
        end
        --Correcting theta and computing its derivative
        theta_correct = actual_theta + c_theta*2*math.pi;
        thetadot = (theta_correct - previous_theta_correct)/(actual_time-previous_time);
        --Storing Previous angles
        previous_alpha = actual_alpha;
        previous_theta = actual_theta;
        alpha_correct = actual_alpha;
        previous_theta_correct = theta_correct;
        previous_time = actual_time;
        --print("Measure. the state is", theta_correct, alpha_correct,thetadot, alphadot);
        return({theta_correct, alpha_correct, thetadot, alphadot});
    end

    -- This function reset the counter for the angle theta. It is called after the swing up.
    function reset_state()
        c_theta = 0;
        previous_theta_correct = -math.atan2(frame2arm[4], frame2arm[8])+0.01;
    end
    
    </code>
</pre>

The functions init_measure_state and measure_state will be called from Matlab through the B0Interface.