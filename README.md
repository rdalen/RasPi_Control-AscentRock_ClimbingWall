# Raspberry Pi control for the Ascent FunRock Climbing wall
This project makes it possible to control your [Ascent FunRock Rotating Climbing wall](https://www.youtube.com/watch?v=9913A6JC2e4) using a Raspberry Pi.
The climbing wall is originally controlled by a windows computer via a serial interface.
There are several [projects](https://github.com/james-schaefer/climbing_wall) that hack the serial protocol to enable control from e.g. a linux computer. 
In this project the PIC controller on the Control&sensor board in the Ascent Rock controlbox is replaces by a interface-adapter to a Raspberry Pi

## The Control Box 
In the control box you will find the following things:
+ A Motordriver Board
+ A Control&Sensor Board
+ An AC/DC converter 12Vdc
+ A Solid state relay
+ A 220Vac Filter
  
![Image](https://github.com/user-attachments/assets/920325ef-27a8-4f6d-b610-b1c8df46e19a)

On the Control & Sensor board there is a PIC controller that provides the local control.
**Hence the position of pin 1**

![Image](https://github.com/user-attachments/assets/00076dc4-0fa1-4953-9959-155c7547caa8)

This PIC Controller can be removed from the board and replaced bij an adapter interface.
**Hence again the position of pin1**

![Image](https://github.com/user-attachments/assets/c846731c-421d-4253-84c1-664b64dce70d)

![Image](https://github.com/user-attachments/assets/e970b072-6869-4e7e-af19-e41e519ca6de)

![Image](https://github.com/user-attachments/assets/c3dd3c93-2739-42d3-b39b-0a7e3bd388ef)


