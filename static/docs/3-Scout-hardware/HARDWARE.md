# 5. Scout Hardware
In this section information about the hardware of the scout can be found.

## 5.1 Sensors
The scout has 2 Tritons and 1 Jupyter.
- The Tritons make images from the surface from which features can be recognized. These features can be recognized by Robins. The Tritons are not able to localize their position in comparison to the origin. 
![Triton](images/_B3A2763.jpg)
- The Jupyter is responsible for absolute localization. This way the features recognized by the Tritons can be translated in to an absolute position. This results in whenever a Robin recognizes the features on the image of the ground with his Triton, he can localize himself based on the absolute position that is linked to that feature by the Jupyter. 
![Jupiter](images/_B3A2759.jpg)
The Tritons and Jupyter are each powered by a power cable and connected to the NUC with a data cable.
![Cables](images/_B3A2762.jpg)
1. The data cable.
2. The power cable.


## 5.2 Power supply
Now we will delve deeper into the scout and its components inside the box. There is 2 main ways for the scout to be powered.
- The batteries.
- Through mains power.
![Batteries](images/_B3A2756.jpg)
Inside the scout a hot swap board is situated which regulates the incoming power. On prio (priority) 1 the mains power is connected, which means whenever the mains power is connected the scout will use this as input. On prio 2 the left front battery is connected and on prio 3 the right front battery.
![Hotswap](images/_B3A2788.jpg)
From the hot swap board the power will go towards the voltage divider, which divides the incoming power over all sensors, NUC and switch.
![Accerion](images/_B3A2787.jpg)

## 5.3 ODrive
The ODrive handles the communication with the encoder and motor of the wheels. It also powers the motors and handles communication with the encoders of these motors. 
![ODrive](images/_B3A2781.jpg)

> **Important:** Make sure the straps around the wheels are tight!

![Wheels](images/_B3A2799.jpg)


## 5.4 NUC & Switch
The NUC is the brain of the scout on which all software is run. The NUC communicates with the ODrive, receives the data from the sensors and runs CX-Appcenter and its apps. The switch connects the sensors with the NUC. This way the data from the sensors can be transferred to the NUC.
![NUC](images/_B3A2772.jpg)
