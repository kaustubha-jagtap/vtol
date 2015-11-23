##Hardware in Loop Simulation
  1. Installation 
  <p><code>git clone https://github.com/PX4/jMAVSim.git</code></p>
  
  2. Install dependencies 
   <p><code>sudo apt-get install ant openjdk-7-jdk openjdk-7-jre</code></p>
  
  3. Install prerequisites and GIT repository initialization
    <p><code>cd jMAVsim</code></p>
    <p><code>git submodule init </code></p>
    <p><code>git submodule update </code></p>
    
  4. Built and run
    <p><code> ant </code></p>
  
  5. Configure PX4 autopilot for HIL setup using either of these methods:
     1.Using QGroundControl: Connect to board, click Setup, click Airframe, check the box in the Simulation selection (last one in list) and pick the type of HIL setup you want from the drop-down. Apply and Restart, Reconnect
     2.By hand: Set parameter SYS_AUTOSTART = 1001, save parameters and reboot
    
  6. Currently jMAVsim is run only after hardcoding the port name in line 110 in<code>Simulator.java</code> in <code>jMAVSim/src/me/drton/jmavsim</code>
     (Port usually is <code>/dev/ttyACM0</code>)
     
  7. Create a copy of <code> px4.launch </code> and name it <code> px4_hils.launch </code>
     Change the default value of two arguments 
     <code> name="fcu_url" default="udp://:14550@localhost:14555"</code>
     <code> name="gcs_url" default="udp://:14557@localhost:14556" </code>
     
  8. Run jMAVSim in HITL mode. Change the serial port to the port of Pixhawk and <b>make sure to have QGroundControl NOT connected to serial.</b> 
    <p><code>java -Djava.ext.dirs= -cp lib/*:out/production/jmavsim.jar me.drton.jmavsim.Simulator -serial /dev/ttyACM0 921600 -qgc </code>
 
  9. Launch newly created <code> px4_hils.launch </code>
  
  10. Arm the quad and start flying :)
