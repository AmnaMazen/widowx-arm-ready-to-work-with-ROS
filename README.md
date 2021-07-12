# widowx-arm-ready-to-work-with-ROS
This file is tutorial to make widowx arm ready to work with ROS


Author name: Amna Mazen Ali

I uploded a video about step by step tutorial on Youtube:
https://www.youtube.com/watch?v=ZblFxjkI5nw&t=122s



# Step 1: Check port number on which arm is connected
 Normally, widowx Arm will be connected to port ttyUSB0 or ttyACM0. To check the port number to which the arm is connected:

* Connect  widowx Arm's USB to your computer and open new terminal
 
$ ls /dev/tty*



* Disconnect the USB and run the command again to check that port number disappears

$ ls /dev/tty*


# Step 2: Upload ardunio "ros" file into Arbotix board

 I will follow this tutorial with some modifications
 https://github.com/RobotnikAutomation/widowx_arm/blob/master/README.md

1- Download Ardunio ide:

$ wget https://downloads.arduino.cc/arduino-1.0.6-linux64.tgz

and extract it in Downloads/ardunio-10.6

2- Download the firmware archives

$ wget https://github.com/trossenrobotics/arbotix/archive/master.zip

Create a new folder inside "Documents" directory and call it "Ardunio"
and extract files into "~/Documents/Ardunio"

3- Run arduino from the folder you extracted it previously

$ cd ~/Downloads/arduino-1.0.6

$ ./arduino

4- Once Arduino IDE is running, change the Sketchbook folder location to
~/Documents/Arduino/arbotixmaster

File->Preferences->Sketchbook Location

5- Change Board to "Arbotix"

Tools->Board->Arbotix

6- Change port to "ttyUSB0"

Tools->Serial Port->/dev/ttyUSB0

7- Choose ros file to upload on Ardunio board of Arbotix Arm

File->Sketchbook->Arbotix Sketches ->ros
Verify + Upload

# Step 3: Create a ROS workspace

$ mkdir -p ~/widowx/src

$ cd ~/widowx/src

$ catkin_init_workspace

$ cd ..

$ catkin_make


# Step 4: Clone package of widowx arm controller
$ cd ~/widowx/src

$ git clone https://github.com/RobotnikAutomation/widowx_arm.git

$ cd ..

$ catkin_make


# Step 5: Creating the udev rule for the device
1- In the widowx_arm_controller/config folder there's the file 58-widowx.rules.
 You have to copy it into the /etc/udev/rules.d folder.

 $ cd src/widowx_arm/widowx_arm_controller/config
 
 $ sudo cp 58-widowx.rules /etc/udev/rules.d

2- You have to set the attribute ATTRS{serial} with the current serial number
of the ftdi device

$ udevadm info -a -n /dev/ttyUSB0 | grep serial

3- Once modified you have to reload and restart the udev daemon

$ sudo service udev reload

$ sudo service udev restart

$ sudo udevadm trigger


# Step 6: Running the controller
$ roslaunch widowx_arm_controller widowx_arm_controller.launch

* You will have this error
"[Errno 2] No such file or directory: '/dev/ttyUSB_WIDOWX'"

* To fix this error: You need to modify "arm.yaml" file inside "widowx_arm_controller" package

* replace "ttyUSB_WIDOWX" with "ttyUSB0"

* Then catkin_make to apply this change and run the controller again

 $ cd ~/widowx
 
 $ catkin_make
 
 $ roslaunch widowx_arm_controller widowx_arm_controller.launch

