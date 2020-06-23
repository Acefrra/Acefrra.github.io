---
    project: furuta
    type: subsect
    section: Connection
    title: B0 CLient Side Enabling (Matlab)
---

This API is loaded by Coppelia, and allows Matlab to connect with the robotic simulator to call built-in or user-defined Coppelia lua functions. In Matlab it is necessary to include in the same folder of the main program the class b0RemoteApi which will instantiate the client. This class also requires some other .dll libraries(since Windows it has been used) or .so libraries (if using Linux) to work.

The constructor is called to create the client:

<pre>
<code>
client=b0RemoteApi('b0RemoteApi_matlabClient','b0RemoteApiAddOn');
</code>
</pre>

This command create the client. The first string is just a symobolic name to the newly created object client, while the second identifies the channel name (see previous step).

After creating the client, it is possible to use the The B0-based remote API functions. Before starting the simulation, we want to enable a syncronous communication between Matlab and Coppelia, otherwise the system will contnue evolving while Matlab computing the control input.

To enable Synchronous operation:

<pre>
<code>
client.simxSynchronous(true); %Enable the synchronous mode
</code>
</pre>

After this command the physic engine will need a trigger from the client before computing the following step. We can safely start the simulation:

<pre>
<code>
client.simxStartSimulation(client.simxDefaultPublisher());
</code>
</pre>

And give the simulator the trigger to go to the first step:

<pre>
<code>
client.simxSynchronousTrigger();
</code>
</pre>

It is possible to call the Coppelia functions mainly in two modes: **blocking** and **non-blocking mode**. We use the non-blocking mode when we don't expect a response from the server(starting the simulation or feeding the control input voltage), so the main can continue execute the code leading to fast performance. The blocking mode is used when a return value is required to execute the subsequent line of code(for example when asking for the state of the system). To call in a non-blocking mode the method <code>client.simxDefaultPublisher()</code> is used, whilst for the blocking mode the method <code>client.simxServiceCall()</code> is called. This method will create a topic over which publish the messages. 

For example, to start the simulation we would do:

<pre>
<code>
client.simxStartSimulation(client.simxDefaultPublisher()); %It starts the simulation
</code>
</pre>

or to feed the voltage Vm:

<pre>
<code>
client.simxCallScriptFunction('input_voltage@Frame',1,Vm,client.simxDefaultPublisher());
</code>
</pre>
<code>client.simxStartSimulation</code> calls a built.in function. In this case the function starts the simulation.

<code>client.simcCallScriptFunction</code>, will call a user-defined lua script. So it is necessary to pass it the name of function and the object parent(the child belongs to) as well as, the identifier to the type of script(1 for Customization Script, 6 for Non-Threaded Script).

To measure the state x it will be run:

<pre>
<code>
state = client.simxCallScriptFunction('measure_state@Dummy',6,-1,client.simxServiceCall());
</code>
</pre>


