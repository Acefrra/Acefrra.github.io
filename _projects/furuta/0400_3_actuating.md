---
    project: furuta
    type: subsect
    section: Building Simulation Scene
    title: Actuating Motor
---

After computing the state, Matlab will be computing the control input that is the voltage to the motor. The voltage must be communicated to Coppelia. So another Coppelia script will be in charge of getting the voltage, and convert it to a torque.

The function will just put the voltage in a global variable

<pre>
<code>
function input_voltage(voltage)
    Vm = voltage
end
</code>
</pre>

The electromechanical convertion depends not only on the voltage but also on the motor angular velocity. So we need to compute it in the sensing code.

<pre>
<code>
local Pendulum
local RotatingArm
local Frame
local arm2pendulum
local frame2arm
local previous_alpha
local previous_theta
local actual_alpha
local actual_theta
local alphadot
local thetadot
local torque
local eta_g, k__g, eta__m, k__t, k__m, r__m

function sysCall_init()
    -- do some initialization here
    Pendulum = sim.getObjectHandle("Pendulum");
    RotatingArm = sim.getObjectHandle("RotatingArm");
    Frame = sim.getObjectHandle("Frame");
    Revolute1 = sim.getObjectHandle("Revolute1");
    arm2pendulum = sim.getObjectMatrix(Pendulum, RotatingArm);
    frame2arm = sim.getObjectMatrix(RotatingArm,Frame);
    previous_time = sim.getSimulationTime();
    previous_theta = -math.atan2(frame2arm[4], frame2arm[8]);
    --Parameter for eletromechanical actuation
    Vm = 0;
    eta__g = 0.85;
    eta__m = 0.87;
    k__g = 70;
    k__t = 0.0076;
    r__m = 2.6;
    k__m = 0.0076;
    Vmax = 10;
    c_theta = 0;
    previous_theta_correct = 0;
end

function sysCall_actuation()
    -- put your actuation code here
    torque = (eta__g*k__g*eta__m*k__t*(Vm-k__g*k__m*thetadot))/(r__m);
    if ((torque == 1/0) or (torque ==-1/0) or (torque~=torque)) then
        torque = 0;
    end
    --print(Vm, torque);
    velocity = sign(torque)*1000; --No limit to the velocity
    sim.setJointForce(Revolute1,math.abs(torque));
    sim.setJointTargetVelocity(Revolute1, velocity);

end

function sysCall_sensing() --This code is just to compute the back EMF
    arm2pendulum = sim.getObjectMatrix(Pendulum, RotatingArm);
    frame2arm = sim.getObjectMatrix(RotatingArm,Frame);
    actual_time = sim.getSimulationTime();
    actual_theta = -math.atan2(frame2arm[4], frame2arm[8]);
    if(actual_theta*previous_theta<0) then
        if (math.abs(previous_theta)>1) then
            (previous_theta))))/(actual_time-previous_time);
            if actual_theta>0 then
                c_theta = c_theta - 1;
            else
                c_theta = c_theta + 1;
            end
        end
    end
    previous_theta = actual_theta;
    theta_correct = actual_theta + c_theta*2*math.pi;
    thetadot = (theta_correct - previous_theta_correct)/(actual_time-previous_time);
    previous_theta_correct = theta_correct;
    previous_time = actual_time;
    print(theta_correct);
end

function sysCall_cleanup()
    -- do some clean-up here
end

-- See the user manual or the available code snippets for additional callback functions and details


function sign(x)
    if x >= 0 then
        return 1
    else
        return -1
    end
end
</code>
</pre>



