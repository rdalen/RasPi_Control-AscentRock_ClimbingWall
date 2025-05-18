
## RasPi2-Climbingwall Configuration
Use device; Raspi2 Model B+ v1.2  
Download Raspberrypi imager and run for windows ; https://www.raspberrypi.com/software.  
Insert MicroSD card 16Mb.  
Hence; To avoid future compatibility issues (eg SenseHAT), select Raspberry pi OS LITE 32b Bullseye.  

Fill in;
Hostname; RasPi2-ClimbingWall  
username; pi  
password; raspberry  
SSID; \<yourSSID>  
password; \<yourPW>  
In SSH tab - use; password authentification


Flash Raspberry Pi OS image;  Raspberry Pi OS Bullseye 32b Lite  
Connect SDCard, Power USB micro  
Power up RasPi & login from monitor/keyboard  

Find out the IP addresses  
`ifconfig`  
And give it a fixed IP addresses in the router  
Reboot  
`sudo reboot`  
and check  
`ifconfig`  
and connect with RasPi (e.g. via the SSH client MobaXterm)  

Expand the filesystem to maximum  
`sudo raspi-config  
menu>Advanced Options >Expand Filesystem`  

Update & Upgrade; Update the package repositories and upgrade all packages  
`sudo apt update`  
`sudo apt upgrade`

### Python

Check python version (python 3.9.2 is preinstalled in RPI OS Bullseye)  
`
python -V
`  
Install PIP (Package Installer for Python)  
`
sudo apt install python3-pip
`  
and check  
`
pip --version
`  
Install git  
`
sudo apt install git
`  

### Node-RED
Install Node-red (or, if Node-RED is already installed, upgrade to the latest version)  
`
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
`  
Start Node-RED  
`
node-red-start
`  
Test  
`
http://<IP>:1880
`  

autostart Node-RED after every boot  
`sudo systemctl enable nodered.service`  
`sudo reboot`  

Test  
`
http://<IP>:1880
`  
add nodes via Manage Palette menu; node-red-dashboard, ui-led  

To define the multiple storage option for context data - so Node-Red will save the context data and make it available after restarts - 
add in  
`
sudo nano "/home/pi/.node-red/settings.js"
`  
the following definition on row 270     
```
contextStorage: {
    default: "memoryOnly",
    memoryOnly: { module: 'memory' },
    file: { module: 'localfilesystem' }
},
```


---
## ZeroTier (see [instruction](https://pimylifeup.com/raspberry-pi-zerotier))

In my.zerotier.com - create a new virtual network AscentRock01  
For AscentRock01, the networkID is \<netwerkID>  

To install ZeroTier directly from the repository, add the GPG key - so download the GPG key first  
`
curl https://raw.githubusercontent.com/zerotier/ZeroTierOne/master/doc/contact%40zerotier.com.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/zerotierone-archive-keyring.gpg >/dev/null
`  

Build the URL for the ZeroTier repository  
`RELEASE=$(lsb_release -cs)`  

`echo "deb [signed-by=/usr/share/keyrings/zerotierone-archive-keyring.gpg] http://download.zerotier.com/debian/$RELEASE $RELEASE main" | sudo tee /etc/apt/sources.list.d/zerotier.list`

update the package list  
`
sudo apt update
`  
install the ZeroTier package  
`
sudo apt install -y zerotier-one
`  

Join the created ZeroTier network  
`
sudo zerotier-cli join \<netwerkID>
`  
check the return massage  
`
200 join OK
`  

Go to the AscentRock01 network in my.zerotier.com  
Select the device RasPi2-ClimbingWall and click the Authorize button  
The Address column gives deviceID within the ZeroTier network - Check on the RasPi  
`
sudo zerotier-cli status
`  

Click the Edit button in the Edit colomn and give it a name/description; RasPi2-ClimbingWall  

The column “Managed IPs”  lists the  IP  in the virtual ZeroTier network to connect to RasPi2-ClimbingWall (may take a few moments)  
The RasPi2-ClimbingWall IP is \<virtualIP>  
Check  
`
sudo zerotier-cli listnetworks
`  

To test; connect in Windows or IPhone ZeroTier app to the Virtual network  
- Node-Red; http://\<virtualIP>:1880  
- Node-Red Dashboard; http://\<virtualIP>:1880/ui  
- SSH session (e.g. SSH client MobaXterm)  
