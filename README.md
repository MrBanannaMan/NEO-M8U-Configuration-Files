# :space_invader: NEO-M8U-Configuration :space_invader:
GNSS Configuration for the NEO-M8U Configuration via .yaml file in ROS using Kumar Robotics Ublox ROS drivers and in Windows via Ublox's U-Center. By completing this "tutorial" you should be able to perform the following:

- [ ] Configure the NEO-M8U Module to track GPS, GLONASS, and Galileo Satellites in Windows 10
- [ ] Poll NMEA 0183 data in Windows and view satellite strength, availability, deviation, etc.
- [ ] (Optional) Map the NMEA data to Google Earth from U-Center *KML file from "Database Export" command in U-Center*
- [ ] Configure the NEO-M8U Module to track GPS, GLONASS, and Galileo Satellites in Linux via Wine
- [ ] Configure the NEO-M8U Module to track GPS, GLONASS, and Galileo Satellites in ROS via sensor_msgs/NavSatFix.h
- [ ] Poll & Bag "NavSatFix" data specified by: http://docs.ros.org/api/sensor_msgs/html/msg/NavSatFix.html
- [ ] (Optional) Map the NavSatFix data to Bing Maps or others depending on API's and time availabe.

## For Configuration in Windows via U-Center:
### Installing U-Center:
Download the latest version of U-Center from Ublox's website by clicking the following link.

https://www.u-blox.com/en/product/u-center-windows - NOTE: Very important - when installing, expand the "Drivers" and check the GNSS driver box and NOT the Windows USB driver box.

Download the Virtual COM Port (VCP) Driver by Ublox from https://www.u-blox.com/en/product-resources/2673The

After installation of the VCP driver, in Device Managers there should be a designated U-Blox COM port which will be used to communicate with the NEO-M8U module.

With U-Center, the NEO-M8U module can be configured three different ways, however we will only explore the SQI flash method for permanent configuration as it is the most useful given our time restraints.

  #### 1. Power & Reset Dependant
  In U-Center, configurations can be applied to the NEO-M8U module such that they retain their values for the time at which the 
  module is powered or until it is either hardware or software reset.
  
  #### 2. Batery Backed RAM (BBR)
  In U-Center, configurations can be applied to the NEO-M8U module such that they retain values independent of the status of 
  external power or any resets. The configuration parameters are retained as long as the battery supply on board is not 
  interrupted.
  
  #### 3. Serial Quad I/O (SQI) Flash
  In U-center, configurations can be applied to the NEO-M8U module such that the parameter values will always retain their 
  given values no matter the status of any power supplies or resets.

### U-Center SQI Configuration
1. Download the Configuration_Ublox_Neo_M8U.txt configuration file from my github. Note to self, this configuration file will be tested and finalized then exported in Windows U-Center then put onto github. The instructions to export this file are found in the U-Center_User_Guide pdf file.
2. Connect the mPCIe NEO-M8U card to the mPCIe to USB adapter and plug into your PC then open U-Center.
3. Click on the Magic Wand at the top toolbar to enable autoset for the Baud Rate. Wait for the "Connection Status" to turn green.

![alt text](http://andrea-toscano.com/wp-content/uploads/2015/05/U-Center-magic-wand.png)

4. On the toolbar, go to Tools -> GNSS Configuration and select File->GNSS. Browse and select the "Configuration_Ublox_Neo_M8U.txt" configuration file downloaded from my github. Make sure the "Store Configuration in BBR/Flash" box is checked!

![alt text](http://andrea-toscano.com/wp-content/uploads/2015/05/U-Center-GNSS-Configuration-300x251.png)

![alt text](http://andrea-toscano.com/wp-content/uploads/2015/05/U-Center-upload-configuration.png)

5. On the toolbar, go to View -> Message View and scroll down to UBX -> CFG.

![alt text](http://andrea-toscano.com/wp-content/uploads/2015/05/U-Center-Message-View-220x300.png)

6. If any other parameters are desired, input them into the UBX -> CFG settings and click "Send" when you are done on the bottom left toolbar. Note that I made this configuration file specifically for this receiver, so be sure that you are aware of what you are changing in the configurations before proceeding.

![alt text](http://andrea-toscano.com/wp-content/uploads/2015/05/U-Center-CFG.png)

7. Back on the U-Center main page, you must go to Receiver -> Action and select "Save Config". If you do not do this step then the configuration settings will not be flashed to the SIQ memory on the NEO-M8U module.

![alt text](http://andrea-toscano.com/wp-content/uploads/2015/05/U-Center-save-configuration.png)


Congratulations! You have just completed configuring the NEO-M8U module:
- [x] Configure the NEO-M8U Module to track GPS, GLONASS, and Galileo Satellites in Windows 10

Cool! :squirrel:

Once connected to the NEO-M8U module's virtual COM port, the auto-bauding and pre-configured file should be setup to begin polling the NMEA data behind the scenes. Given that you are outdoors, you should start to see satellites being picked up by the receiver, their strengths, and eventually a 2D or a 3D (internal IMU in NEO-M8U is 3D) Fix should take place.
Simply play around with the GUI in U-Center to view the satellite strength, availability, deviation, and more.

Et Voila! You have just completed:
- [x] Poll NMEA 0183 data in Windows and view satellite strength, availability, deviation, etc.

### Map the NMEA 0183 data to Google Maps in U-Center
This section is optional but it allows us to visualise where the NEO-M8U thinks we are and can be shown on google Maps as well as google Maps satellite option. Note that an internet connection is required to do this in REAL TIME. In offline mode, the NMEA data must be saved to a log file which can later be opened in Google Maps following the same steps as in online mode.

1. Open U-Center with the NEO-M8U receiver connected to the PC via the mPCIe to USB adapter and connected to the virtual COM port.
2. On the toolbar, File->Database Export->Google Map Html...
Save the .html file anywhere you'd like and open the file to view the coordinates in google Maps.
3. If you are exporting a datalog saved offline then you need to make sure that you have opened the log, pressed play, and let the log file finish it's entire recording session as shown by the progress bar on the toolbar.

You have just completed:
- [x] Map the NMEA data to Google Earth from U-Center *KML file from "Database Export" command in U-Center*

## For Configuration in Linux :penguin: via Wine (Windows Emulator)
Ensure that the Linux operating system is Ubuntu and is a version greater than or equal to v16. Also ensure that the architecture your processor uses is NOT ARM. Wine does not support ARM architecture! F

Follow these steps:

1. Download the Wine Binary Package by following this link and proceeding through the "Online" steps.

https://wiki.winehq.org/Ubuntu

The terminal inputs should look like the following for a 64-bit Ubuntu system:

sudo dpkg --add-architecture i386
wget -nc https://dl.winehq.org/wine-builds/Release.key
sudo apt-key add Release.key
sudo apt-add-repository https://dl.winehq.org/wine-builds/ubuntu/

sudo apt-get update
sudo apt-get install --install-recommends winehq-stable

2. Run the command: winecfg
3. The Wine configuration window will open. Accept any downloads and installs to mono and gecko packages.
4. Under the "Applications" Tab, select "Windows Version" and you MUST select Windows 10. U-Center does not support any other Windows OS. Save the configuration settings and exit the configuration window.
5. Download and install the latest version of U-Center from the following link:

https://www.u-blox.com/en/product/u-center-windows

6. Read the note below first, then run the commands:

sudo adduser YOUR_USERNAME dialout
cd ~/.wine/dosdevices
ls
ln -s/dev/ttyACM1 com1
sudo reboot

Note: YOUR_USERNAME should be the name you use on your PC, and most importantly the com port may not be com1. The com port of the NEO-M8U module must be checked prior to these steps to ensure you are on the correct one.

7. After reboot, open U-Center and connect the NEO-M8U module to your PC. 
8. Continue off of Step 3. in the Windows U-Center SQI Configuration section above.
9. If there is no communication between the NEO-M8U and U-Center, run the following commands:

unlink com1
ln -s /dev/ttyACM1 com1

Congratulations! You should now be able to,
- [x] Configure the NEO-M8U Module to track GPS, GLONASS, and Galileo Satellites in Linux
Cool! :squirrel:

## For Configuration in Linux :penguin: via ROS
Ensure that the ROS system you are working with is the Kinetic distribution. 

### For the NEO-M8T (Timing GNSS Module)
1. Clone this repo into your catkin workspace and ensure to perform a catkin_make command for the repo: https://github.com/flynneva/ublox.git 
2. Go into ~/catkin_ws/src/ublox/ublox_gps/src and open node.cpp 
3. On line 124, change: nh->param("device", device_, std::string("/dev/ttyACM0")); to nh->param("device", device_, std::string("/dev/ttyACM1"));
4. On line 134, change: getRosUint("uart1/baudrate", baudrate_, 9600); to getRousUint("uart1/baudrate", baudrate_, 115200);
5. Go into ~/catkin_ws/src/ublox/launch and open ublox_device.launch
6. Replace line 4 with: <arg name="node_name" default="ublox_gps"
7. Replace line 5 with: <arg name="param_file_name" default="NEO-M8U"
8. Download the NEO-M8U.yaml configuration file in my github and place it into the directory: ~/catkin_ws/src/ublox/ublox_gps/config
8. Do a ROSlaunch of the node: roslaunch ublox_gps ublox_device.launch
9. In another terminal, poll the NMEA latitude and longitude GNSS data from: rostopic echo /ublox_gps/fix - This topic outputs the ROS file type of sensor_msgs/NavSatFix which contains latitude, longitude, altitude, etc.

### For the NEO-M8U (High-Precision GNSS Module)
1. Clone this repo into your catkin workspace and ensure to perform a catkin_make command for the repo: https://github.com/KumarRobotics/ublox.git
2. Follow steps 2. through 9. for the NEO-M8T Module in the section above to configure and poll the sensor_msgs/NavSatFix data

You are now able to:
- [X] Configure the NEO-M8U & NEO-M8T Module to track GPS, GLONASS, and Galileo Satellites in ROS via sensor_msgs/NavSatFix.h

### Node Topics and Important Information
#### Latitude & Longitude
As expressed in the configuration tutorial, by echoing the /ublox_gps/fix topic we obtain the latitude, longitude, altitude and also the STATUS of the fix. When the status is '-1' then the receiver has NO fix and therefore has NO information and must continue to search for satellites. When the status is '+1' the receiver has found a fix and will output valid positioning information. By echoing this topic, you will see the following on the terminal:

#### Internal IMU
To poll the receiver for the internal IMU values as well as velocity, you can echo the /ublox_gps/fix_velocity topic.

#### TIMESTAMP
To determine the TIME of the DAY and get a TIMESTAMP for when the location or stereonet was taken, we have to echo a different topic: /ublox_gps/navclock

The significance of the published values are:
iTOW: (Unit - ms) GPS Time of the Week - This is the important one. See the note below on how to use this value

clkB: (Unit - ns) Clock Bias - See notes below

clkD: (Unit - ns/s) Clock Drift - See notes below

tAcc: (Unit - ns) Time Accuracy Estimate - See notes below

fAcc: (Unit - ps/s) Frequency Accuracy Estimate

GPS Time of the Week - This is the CALCULATED time on the receiver. It is typically very accurate to within the ms. It will ALWAYS report the UTC time (standard GPS time).

Clock Bias - This is the ERROR between the local time and the calculated time. As mentioned above, typically within the ms.

Clock Drift - This is the RATE at which the Clock Bias or error is changing.

Time Accuracy Estimate - 














