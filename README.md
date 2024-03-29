# install-xavier-nx
In our case the nvidia has a eMMC of 16 Gb and NVMe SSD of 120 Gb. The original boot is from the eMMC but if we use this setting it will be quickely full when installing libraries and therefore we will boot and use the SSD. 

We will use a jumper cable and usb micro to usb cable to flash the xavier nx.

first install SDK manager on a host pc or laptop. download .deb file for ubuntu from,
https://developer.nvidia.com/nvidia-sdk-manager
Now install the manager,
```
cd ~/Downloads/
sudo apt install ./sdkmanager_1.8.4-10431_amd64.deb
```
After it is installed,
```
sdkmanager
```
you will have to login to nvidia to go further and connect your laptop/pc with USB to the mini usb port of the jetson module. follow the instruction in the manager, most is easy to follow. Select manual setup and follow the instruction, you will need to use the jumper or jumper cable.
Important: select storage device NVMe (the ssd).
https://docs.nvidia.com/sdk-manager/install-with-sdkm-jetson/index.html

Now after flashing it will get stuck at installing components. just stop the installation and unplug the powercable from the Xavier. 
Now we will start all over again but select storage device eMMC!! both of the storage devices need the same version in our case Jetpack version 5.02. Afterwards it will again complain about IP adress, just let it rest and don't exit the install. disconnect power cable and now boot the xavier normally, when in the beginning you see the nvidia logo press Esc key to get into the system settings. We need to modifiy the boot settings and change the priority of the boot locations. 
```
Boot maintainance manager -->> Change boot order -->> Select the order and press enter -->> move the SSD to the top
```
Now save the settings and return to booting. The jetson will boot from the SSD card everytime you boot up. Now that you are logged in, connect to the wifi and continue with the process on the laptop of the install. This will take some time.

When the install is ready you can exit the SDK manager and restart the Jetson Xavier. All should be up and running now.


# Changing to better cooling solution
first install nano
```
sudo apt install nano
```
and now change the parameter in the fan configuration,
```
sudo nano /etc/nvpower/nvfancontrol/nvfancontrol_p2888.conf
```
And change the setting
```
FAN_DEFAULT_PROFILE quiet
```
To,
```
FAN_DEFAULT_PROFILE cool
```

You can also change the parameters to your liking.

NOTE: If the fan only operates at max speed after startup use,
```
sudo systemctl restart nvfancontrol.service
```

# UART
When starting up the jetson is going to communicate through UART when it is connected.
this interferes with the px4 module, so we have to disable the debug console trhough UART
```
systemctl stop nvgetty
systemctl disable nvgetty
udevadm trigger
```
now restart and configuration should be fine.

# Terminal through micro-USB connection

we use GTKterm to create a serial console connection to the jetson from a laptop
```
sudo apt install gtkterm
```
Start the jetson and connect the USB to the laptop and micro-USB to jetson. Now start gtkterm:
```
sudo gtkterm
```
In the console go to configurations -> port and set the correct settings:
- port: for example /dev/ttyACM0 but this can differ, you can select one from the drop down menu.
- Baudrate: 115200
- Parity: none
- Bits: 8
- Stopbits: 0
- Flow Control: none
press ok and then it will ask for login and password of the jetson system. After correctly putting in the cridentials you will see that you are connected to a terminal on the jetson.
