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

Position of the adapter interface on the Control&Sensor board when the PIC Controller is removed.\
**Watch again the position of pin1**
![Image](https://github.com/user-attachments/assets/c3dd3c93-2739-42d3-b39b-0a7e3bd388ef)

To read the ON/OFF status of the incline motor, the small SMD Diode CR5 must be bridge by a wire (or removed and replaced bij een wire).
![Image](https://github.com/user-attachments/assets/ff3f1cff-c8b7-4272-9f4c-2c61ecfadbee)

## The Raspberry Pi
Interface on the RasPi side of the interface cable.
![Image](https://github.com/user-attachments/assets/f44d5229-a0ef-4c90-9e75-f0dc811b8678)

The RasPi has small magnets on the back, so that it can be easily attached in the Control Box (if the magnetic force is insufficient, a second set of magnets can be glued to the wall of the control box).
![Image](https://github.com/user-attachments/assets/847f3481-7357-4767-9331-9fa5581eb1a5)

### Software
The control is made in Node-Red. For now, the control is very basic
+ It reads the three Test buttons on the Control&Sensor board to test the Incline Motor and the Speed Motor:
  + The Incline + Button (the incline motor with 500ms delay to set the direction relay (K3) first)
  + The Incline - Button (with 500ms delay to set the direction relay (K3) first)
  + The Speed Button (25% of the max speed)
![Image](https://github.com/user-attachments/assets/7d8e421f-052d-4e50-84a8-b23413c0e593)
+ An User interface;
  +  Slider to control the speed of the speedmotor and stop button
  +  Incline + and Incline - buttons to control the Incline motor and a stop button
  +  Indication that the incline motor is running
![Image](https://github.com/user-attachments/assets/666d28d2-0700-41e1-90de-c6a6ea60532c)

## Commissioning of the Climbing wall
To put the climbing wall in operation
1. make the hardware modifications
  - replace the PIC Controller (A2) bij the adapter interface (Please note the position!)
  - Bridge or replace the diode CR5 by a wire
2. 
  
