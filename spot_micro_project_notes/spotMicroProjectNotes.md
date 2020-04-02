# Spot Micro Project Notes

* [Raspberry Pi Raspbian Installation Notes](#raspberry-pi-raspbian-installation-notes)
* [Raspberry Pi Ubuntu Notes](#raspberry-pi-ubuntu-notes)
* [Raspberry Pi GPIO Diagram](#raspberry-pi-gpio-diagram)
* [Useful ROS Commands](#useful-ros-commands)

## Raspberry Pi Raspbian Installation Notes
Following instructions on http://wiki.ros.org/ROSberryPi/Installing%20ROS%20Kinetic%20on%20the%20Raspberry%20Pi

* Set IP Address as static: 192.168.1.80
* Run update: `sudo apt-get update`
* Install Samba `sudo apt-get install samba`
    * File server, to allow sharing directory accross network. 
    * Share Catkin workspace source folder, edit /etc/samba/smb.conf and at the bottom add the following:
        ```
        [catkin_ws_src]
        comment = Samba on pi
        path = /home/pi/ros_catkin_ws/src/
        read only = no
        browsable = yes
        ```
    * Connect in ubuntu by opening file manager, find `Connect to Server` and enter: `smb://192.168.1.80/catkin_ws_src`
    * Consider replacing with other file share in the future
* Install libi2c libraries: `sudo apt-get install libi2c-dev`
* Clone i2c ROS Librarty from gitlab into ros catkin workspace src directory (https://gitlab.com/bradanlane/ros-i2cpwmboard):   
    ```
    cd ~/ros_catkin_ws/src
    git clone https://gitlab.com/bradanlane/ros-i2cpwmboard.git
    ```
* Cd back to ros_catkin_ws and run `catkin_make`, or if that doesnt work, run `catkin_make_isolated`
    * May need to change group and owner of build_isolated and other directories in ros_catkin_ws, do so via:
        ```
        sudo chown -R pi build_isolated
        sudo chgrp -R pi build_isolated
        ```
* Problem with i2c durign compilation... trying premade ubuntu image instead

## Raspberry Pi Ubuntu Notes
* Using prebuilt image from https://downloads.ubiquityrobotics.com/
* Changed hostname to UbuntuRosPi
* Password is 1234
* Run update: `sudo apt-get update`
* Install Samba `sudo apt-get install samba`
* File server, to allow sharing directory accross network. 
    * Share Catkin workspace source folder, edit /etc/samba/smb.conf and at the bottom add the following:
        ```
        [catkin_ws_src]
            comment = Samba on ubuntu
            path = /home/ubuntu/catkin_ws/src/
            read only = no
        browsable = yes
        ```
    * Connect in ubuntu by opening file manager, find `Connect to Server` and enter: `smb://ubunturospi.local/catkin_ws_src`
    * Consider replacing with other file share in the future
* Install libi2c-dev via `sudo apt install libi2c-dev`
* cd to catkin src directory, clone ros i2c library (from https://gitlab.com/bradanlane/ros-i2cpwmboard/-/tree/master), and run catkin_make
    ```
    cd ~/catkin_ws/src
    git clone https://gitlab.com/bradanlane/ros-i2cpwmboard.git
    cd ..
    catkin_make
    ```
* cd to catkin_ws/ros-i2cpwmboard/launch, and edit i2cpwm_node.launch
    * Remove launch prefix section "sudo -E"
* Connect PCA9685 and run ros i2c node via `rosrun i2cpwm_board i2cpwm_board`


## Raspberry Pi GPIO Diagram
![Raspberry Pi 3 GPIO Diagram](assets/rpi_3_gpio_diagram.png)


## Useful ROS Commands
* See ros topic list: `rostopic list`
* See info about topic: `rostopic info /<topic_name>`, e.g.: `rostopic info /servos_absolute`
* Show structure of message: `rosmsg show i2cpwm_board/ServoArray`
* 