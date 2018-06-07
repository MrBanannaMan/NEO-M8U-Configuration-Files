# NEO-M8U-Configuration
GNSS Configuration for the NEO-M8U Configuration via .yaml file in ROS using Kumar Robotics Ublox ROS drivers and in Windows via U-Center.

# For Configuration in Windows via U-Center
The NEO-M8U module can be configured in Windows three different ways:
  # 1. Power & Reset Dependant
  In U-Center, configurations can be applied to the NEO-M8U module such that they retain their values for the time at which the 
  module is powered or until it is either hardware or software reset.
  
  # 2. Batery Backed RAM (BBR)
  In U-Center, configurations can be applied to the NEO-M8U module such that they retain values independent of the status of 
  external power or any resets. The configuration parameters are retained as long as the battery supply on board is not 
  interrupted.
  
  # 3. Serial Quad I/O (SQI) Flash
  In U-center, configurations can be applied to the NEO-M8U module such that the parameter values will always retain their 
  given values no matter the status of any power supplies or resets.



# For Configuration in Windows via ROS
