# Raspberry Pi control for the Ascent FunRock Rotating Climbing Wall

![Image](https://github.com/user-attachments/assets/8acd1aac-b8fc-4ed2-9202-45d1c159e223)

This project makes it possible to control your [Ascent FunRock Rotating Climbing Wall](https://www.highwaygames.com/arcade-machines/fun-rock-8735/)
using a Raspberry Pi. 

The climbing wall is originally controlled by a windows computer via a serial interface.
There are several [projects](https://github.com/james-schaefer/climbing_wall) that hack the serial protocol to enable control from e.g. a linux computer.

In this project the PIC controller on the Control & Sensor Board in the Ascent Rock Control Box is replaced by a interface-adapter to a Raspberry Pi. 
See here a little [demo](https://youtube.com/shorts/axWC5yzVSEU) with a simple Lego model of the climbing wall ;-)

Since I couldn't find electronic schematics, I made these myself through reverse engineering. So there may be some errors in the diagrams!

## The Control Box 
In the Control Box you will find the following things:
+ A [Motordriver Board](docs/Ascent%20Rock%20climbing%20wall%20-%20motordriverBoard.pdf)
+ A [Control & Sensor Board](docs/Ascent%20Rock%20climbing%20wall%20-%20control%26sensorBoard.pdf)
+ An AC/DC converter 12Vdc
+ A Solid state relay
+ A 220Vac Filter (dismounted in this picture)
  
![Image](https://github.com/user-attachments/assets/920325ef-27a8-4f6d-b610-b1c8df46e19a)

On the Control & Sensor Board the main component is a PIC controller that provides the local control.
**Watch the position of pin 1**
![Image](https://github.com/user-attachments/assets/00076dc4-0fa1-4953-9959-155c7547caa8)

## Hardware modifications
The following [Hardware modifications](docs/Ascent%20Rock%20climbing%20wall%20-%20control%26sensorBoard%20-%20modification.pdf) must be made:
1. The PIC Controller (A2) must be removed from the board (pulled out its socket) and replaced by an adapter interface. Pin 1 is indicated.\
Each of the three outputs has a LED indicator to check the RasPi output signals.
![Image](https://github.com/user-attachments/assets/c846731c-421d-4253-84c1-664b64dce70d)
![Image](https://github.com/user-attachments/assets/e970b072-6869-4e7e-af19-e41e519ca6de)
![Image](https://github.com/user-attachments/assets/e556ef24-e326-4583-9149-f25a6315d7b8)<br/>
Place the adapter interface in the empty PIC Controller socket.\
**Watch again the position of pin 1**
![Image](https://github.com/user-attachments/assets/c3dd3c93-2739-42d3-b39b-0a7e3bd388ef)

2. To let the RasPi read the ON/OFF status of the incline motor, the small SMD Diode CR5 must be bridge by a wire (or removed and replaced by a wire).
![Image](https://github.com/user-attachments/assets/ff3f1cff-c8b7-4272-9f4c-2c61ecfadbee)
![Image](https://github.com/user-attachments/assets/869d436e-7250-483d-8569-17e05c424b0a)

3. The (orginal) climbing wall has Safety sensors connected to J7 (and J11) of the Control&Sensor board.\
To test the motors, pin 1 and pin 2 of J7 must be bridged by a jumper or wire  so that the solid-state relay can supply power to the motors
![Image](https://github.com/user-attachments/assets/ae8c4b25-6c92-446a-ac79-8f18d6242598)
![Image](https://github.com/user-attachments/assets/216d6d0c-3149-475c-9b34-b506e2e61128)

## The Raspberry Pi
The RasPi configuration is given [here](docs/RasPi_configuration_description.md)  

In the picture you see the interface at the RasPi side of the interface cable.
![Image](https://github.com/user-attachments/assets/f44d5229-a0ef-4c90-9e75-f0dc811b8678)

The RasPi has small magnets at the back, so it can be easily attached in the Control Box (if the magnetic force is insufficient, the second set of magnets can be glued to the wall of the control box).\
![Image](https://github.com/user-attachments/assets/b9382118-2c62-4786-b26a-6eab785ae114)

![Image](https://github.com/user-attachments/assets/847f3481-7357-4767-9331-9fa5581eb1a5)

## Configure WIFI on the RasPi
WIFI is pre-configured on your RasPi. If this does not work, see this [description](docs/RasPi_WIFI_config.md)  

To check that the WiFi connection has been established correctly type type in  
`ifconfig wlan0`  
to see the IP address
![Image](https://github.com/user-attachments/assets/748777c5-0e88-4cd4-8c2e-074695c4cb0c)

Next; give the RasPi a static IP adress in your Router (e.g. in the DHCP settings-menu make a IP reservation) and Reboot the RasPi  
`sudo reboot`  
and check the IP again. Now you can access the RasPi wireless. Any SSH client can be used such as [PuTTY](https://www.putty.org/) on Windows  

### Software
The control is made in Node-Red. For now, the control is very basic.\
Node-red can be accessed via a browser at address: `http://<IP-address>:1880`\
It reads the three Test buttons at the Control & Sensor Board to test the Incline Motor and the Speed Motor:
  + The Incline + Button (the incline motor has a 500ms delay to set the direction relay (K3) first)
  + The Incline - Button (with 500ms delay to set the direction relay (K3) first)
  + The Speed Button (25% of the max speed and with 500ms delay (oopsy delay))

![Image](https://github.com/user-attachments/assets/7d8e421f-052d-4e50-84a8-b23413c0e593)

Node-Red provides the user interface too. The Node-red Dashboard can be accessed via a browser at address: `http://<IP-address>:1880/ui`\
The dashboard provides:
  -  A Slider to preset the speed of the speedmotor and ON/SET and STOP buttons to control the Speedmotor
  -  Incline + and Incline - buttons to control the Incline motor and a stop button
  -  Indication that the incline motor is running

![Image](https://github.com/user-attachments/assets/0995ece4-e2e1-4943-82c9-3606179fcacc)

**Caution! Because the software does not include any safety feature, the <ins>only</ins> safety measures to stop the incline motor are two small mercury switches (S6 & S7) at the Control & Sensor Board that measure the end-positions of the Control Box.**

### Maintenance & Support
Installed on the RasPi is a ZeroTier-Server which allows secure remote login to the RasPi for Maintance and Support.  
In the ZeroTier Server tab in the Node-Red dashboard, it is possible to turn the ZeroTier-Server ON and OFF so that full access control lies with local users of the Climbing Wall
![Image](https://github.com/user-attachments/assets/6e4daf0a-986d-4880-b6a0-805059118823)

## Commissioning of the Climbing wall
To put the Climbing Wall in operation:
1. Make the hardware modifications first:
    - Replace the PIC Controller (A2) bij the adapter interface (Please note the position!)
    - Bridge or replace the diode CR5 on the Control & Sensor Board by a wire
    - Bridge pin 1 and pin 2 of the Safety sensors connector J7 at the Control & Sensor Board.
2. Connect the motors to the Motordriver Board. If it turns out later that they are rotating in the wrong direction, swap the connections.
3. Power up the RasPi (it takes a few minutes to boot up).
4. Connect the 220Vac mains to the Control Box. If the pin 1 and pin 2 of the Safety sensor connector is correctly bridged the green LED DS3 will light up. So check this before continue.
5. Try to operate the Incline en Speed motors by pressing the Test Buttons on The Control & Sensor Board longer then 500ms.
6. Test the two end-positions of the incline motor. The mercury swiches must stop the incline motor at its end-positions (hardwired safety mechanism)
7. Open in Windows the browser and open the Node-Red Dashboard `http://<ip-address>:1880/ui` to operate the Incline en Speed motors from the screen.


[Happy climbing](https://www.youtube.com/watch?v=9913A6JC2e4)!

## Safety & Disclaimer
The maintainers of this repository disclaim any responsibility for damage, harm or injuries made by using the information from this repository.

Use at your own risks. For more information we kindly refer you to attached [LICENSE.md](LICENSE.md).

- Do not modify the original boards in any way if you do not understand the end goal or original purpose of the components
- Do not override any safety mechanisms
- Do not change the mounting position of the Control Box (because the mercury end-position swiches are a part of the safety mechanism)
- Do not allow for external network access: provide proper network security to ensure no unwanted commands are sent to the Climbing Wall
