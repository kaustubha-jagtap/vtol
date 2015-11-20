### Mavros + Mocap + QgroundControl

1. Install latest mavros by clonning the latest mavros version.
  * Change directory to catkin_ws/src and enter following command
  
    <code>$git clone https://github.com/mavlink/mavros.git </code>
  * Build Mavros using following commannd to make 'pixhawk' as the default dialect (Necessary to connect to PX4 autopilot)
    
    <code>$catkin_make -DMAVLINK_DIALECT=pixhawk </code>

2. Install the 'Mocap_optitrack' package to receive data from Mocap system.
  * Enter following command to get the package  
    
    <code>$git clone https://github.com/ros-drivers/mocap_optitrack.git</code>
  * Use the following wiki to set up the mocap system on windows
    
    http://wiki.ros.org/mocap_optitrack
  
  * Make sure that broadcasting option for rigid body is set to 'True'
  
  * build the package using catkin_make command in your catkin workspace
  
  * Launch mocap to receive data from Mocap system using following command.
    
    <code>$roslaunch mocap_optitrack mocap.launch  </code>
  * use <code> rostopic list </code> to check out the topics published by mocap package.
  
  * Make necessary changes to <code> mocap_optitrack/config/mocap.yaml </code> 
    
    1. remap <code> /Robot_1/pose </code> topic to <code> /mavros/vision_pose/pose </code> by replacing the appropriate line

3. Qground Control and Mavros connect to Pixhawk using serial port. By default, You can connect to either one of these.
  Follow step below to connect mavros and QGC( QgroundControl) simultaneously.
  
  * Change second line <code> px4.launch </code> in <code> /mavros/mavros/launch/px4.launch </code>
     change the <code>gcs_url</code> argument default to <code>default="udp://:14556@192.168.150.2:14550" </code>
     (make sure to change the IP address to the machine IP address)
  
  * Now PX4 autopilot can connect using mavros by launching <code> roslaunch mavros px4.launch </code> and QGC can connect
to autopilot using Default UDP link  

##Using Mocap system to estimate position:

1. Open Motive instace on the windows machine.

2. Create rigid body and start broadcasting using the streaming pane tool.

3. Make sure to change the IP address for multicast to the linux machine which connects to autopilot using mavros.

4. Once the remap is successful ( You should see the pose data on <code>/mavros/vision_pose/pose</code> )

5. make sure that vision_pose_estimate plugin is not blacklisted.

6. if everything is set correctly, px4 should sends following message:
  <code> FCU: [inav] VISION estimate valid </code>

7. FCU will use this vision_estimate and you can echo <code> /mavros/local_position/local</code> topic to see what FCU is sending it's local position to be.

8. If everything is set correctly and vision_estimate is not accepted by FCU,
  make sure to increase weights for the vision parameters in POSITION_ESTIMATOR_INAV(which you can change in QGC)
