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
