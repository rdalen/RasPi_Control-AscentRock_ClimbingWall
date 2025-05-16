# Raspberry Pi control for the Ascent FunRock Climbing wall
This project makes it possible to control your [Ascent FunRock Rotating Climbing wall](https://www.youtube.com/watch?v=9913A6JC2e4) using a Raspberry Pi.
The climbing wall is originally controlled by a windows computer via a serial interface.\
There are several [projects](https://github.com/james-schaefer/climbing_wall) that hack the serial protocol to enable control from e.g. a linux computer. 
In this project the PIC controller on the Control&sensor board in the Ascent Rock controlbox is replaces by a interface-adapter to a Raspberry Pi

## The Control Box 
In the control box you will find the following things:
+ A Motordriver Board
+ A Control&Sensor Board
+ An AC/DC converter 12Vdc
+ A Solid state relay
+ A 220Vac Filter (dismounted on the picture)
  
![Image](https://github.com/user-attachments/assets/920325ef-27a8-4f6d-b610-b1c8df46e19a)

On the Control & Sensor board there is a PIC controller in a socket that provides the local control.
**Watch the position of pin 1**
![Image](https://github.com/user-attachments/assets/00076dc4-0fa1-4953-9959-155c7547caa8)

## Hardware modifications
This PIC Controller (A2) must be removed from the board (pulled out its socket) and replaced bij an adapter interface. Pin1 is indicated
The outputs has a LED indicator
![Image](https://github.com/user-attachments/assets/c846731c-421d-4253-84c1-664b64dce70d)

![Image](https://github.com/user-attachments/assets/e970b072-6869-4e7e-af19-e41e519ca6de)

![Image](https://github.com/user-attachments/assets/e556ef24-e326-4583-9149-f25a6315d7b8)

Position of the adapter interface on the Control&Sensor board when the PIC Controller is removed.\
**Watch again the position of pin1**
![Image](https://github.com/user-attachments/assets/c3dd3c93-2739-42d3-b39b-0a7e3bd388ef)

To read the ON/OFF status of the incline motor, the small SMD Diode CR5 must be bridge by a wire (or removed and replaced bij een wire).
![Image](https://github.com/user-attachments/assets/ff3f1cff-c8b7-4272-9f4c-2c61ecfadbee)

![Image](https://github.com/user-attachments/assets/869d436e-7250-483d-8569-17e05c424b0a)

## The Raspberry Pi
Interface on the RasPi side of the interface cable.
![Image](https://github.com/user-attachments/assets/f44d5229-a0ef-4c90-9e75-f0dc811b8678)

The RasPi has small magnets on the back, so that it can be easily attached in the Control Box (if the magnetic force is insufficient, a second set of magnets can be glued to the wall of the control box).
![Image](https://github.com/user-attachments/assets/847f3481-7357-4767-9331-9fa5581eb1a5)

### Software
The control is made in Node-Red. For now, the control is very basic.\
Node-red can be accessed via a browser at address: `http://<ip-address>:1880`
It reads the three Test buttons on the Control&Sensor board to test the Incline Motor and the Speed Motor:
  + The Incline + Button (the incline motor has a 500ms delay to set the direction relay (K3) first)
  + The Incline - Button (with 500ms delay to set the direction relay (K3) first)
  + The Speed Button (25% of the max speed and with 500ms delay (oopsy delay))

![Image](https://github.com/user-attachments/assets/7d8e421f-052d-4e50-84a8-b23413c0e593)

Node-Red provides a user interface too. Node-red Dashboard can be accessed via a browser at address:: `http://<ip-address>:1880/ui`\
The dashboard provides:
  +  A Slider to control the speed of the speedmotor and stop button
  +  Incline + and Incline - buttons to control the Incline motor and a stop button
  +  Indication that the incline motor is running

![Image](https://github.com/user-attachments/assets/d813da7f-671f-40a0-8b89-f332a3d1300d)


## Configure WIFI on the RasPi
To get access to he RasPi the WIFI credentials must be entered first.
- Connect a HDMI Monitor and a USB-keyboard (or IR keyboard with USB Dongle) to the RasPi
- Power up the RasPi and wait a few minutes to boot up. Log in with `username: pi | password: raspberry`
- Open Raspi-Config `sudo raspi-config`
![Image](https://github.com/user-attachments/assets/a1c9d716-3f1b-4cd5-bf11-062dbb230d4a)
- Select System Options and press Enter
![Image](https://github.com/user-attachments/assets/a823daf3-3cf0-4b75-8198-a495e948614d)
- Select Wireless LAN and then follow the on-screen instructions to enter your network’s SSID and password. When you’re done, select “Finish” on the main menu to close Raspi-Config.
- Finally, reboot the Raspberry Pi to apply the settings we’ve just changed.

### Test the WIFI connection
To check that the WiFi connection has been established correctly type type in: `ifconfig wlan0` to see the IP address
![Image](https://github.com/user-attachments/assets/748777c5-0e88-4cd4-8c2e-074695c4cb0c)
Next give the RasPi a static IP adress in your Router (eg in the DHCP settings-menu make a IP reservation) and Reboot the RasPi `sudo reboot` and check the IP again. Now you can access the RasPi wireless by any SSH client can be used such as [PuTTY](https://www.putty.org/) on Windows  

## Commissioning of the Climbing wall
To put the climbing wall in operation:
1. Make the hardware modifications first:
  - Replace the PIC Controller (A2) bij the adapter interface (Please note the position!)
  - Bridge or replace the diode CR5 by a wire
2 - Power up the RasPi and wait a few minutes to boot up.
3. Try to operate the Incline en Speed motors by the Test Buttons on The Control&Sensor board
4. Open in Windows the browser and open the Node-Red Dashboard `http://<ip-address>:1880/ui` and operate the Incline en Speed motors
