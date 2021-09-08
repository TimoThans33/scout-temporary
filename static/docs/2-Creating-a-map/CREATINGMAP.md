# 2. Creating a map
To obtain information about the environment (a map) with the scout, a couple of things need to be done:

## 2.1 Turn on the scout
Before using the scout, make sure you add full batteries to the battery holders of the scout and press the <span style="color:green">green</span> power button.

## 2.2 Connect to WIFI
To use a scout, you need to connect your laptop and scout to the same wifi. One could connect his laptop to the hotspot of the scout or connect the scout and laptop to the same wifi. 
> **Important note:** The IP adress of your scout is always: 192.168.8.(**Scout No. + 10**)

## 2.3 Open Autonomous Mapping Tool (AMT) 
The Autonomous Mapping System (AMT) is an online interface where you can easily manage and control the mapping of your scout. The AMT can be accessed through your webbrowser using the address: _"scout-ip":8800_.

## 2.4 Upload Instructions on the (AMT) 
The mapping plan can be downloaded from ... and can be uploaded on the AMT via the open a file button.

![OpenFile](media/OpenFile.png)

## 2.5 Press play 
Press the play button in the AMT to let the scout start with mapping.
> **Note:** Empty battery case: If during a mapping process the battery gets empthy, the battery can be easily replaced by a new one and the mapping will be proceeded. The scout will initially drive back to the origin of placement and then continue the mapping process.
> **Emergency button:** If the scout must be manually stopped right away, one can immediately shut down the scout using the red emergency button on the scout scout.

## 2.6 Download Map
 When the Scout has finishes mapping, the data from the Scout can be downloaded. Since, the scout has two sensors (left and right) the data is converted on the GUI into one map through an algoritm into an amf file.

## 2.6 Virtualize field in GUI
GUI is an online interface where you can create a virtual map for the Scout you are using. Within this virtualized map you use the coordinates of the measured outlines, corners and obstacles of your field. With this virtualized map GUI can automatically generate a plan for the Scout to drive & collect data of the environment.  

XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX














1. Virtually place your Scout in origin of the field in GUI, facing towards the positive y-axis in GUI. This can be done by clicking on the scout with the right mouse button and change the origin into: x = 0, y = 0, angle = 1.57 rad (since 1,57 = 0.5pi)). 
> **Note:** use . and not , !
2. Virtually write down the measured dimensions of the field in GUI. (As an example: The most basic map is a simple rectangle type of field of 10m X 10m, with clockwise coordinates: [5,-5], [5,-5], [-5,-5], [-5,5], with origin (0,0,0).)
> **Rule of thumb**: Main Branch = y / north-south axis 
3. Virtually write down the coordinates of the obstables you measured in the GUI. As for the map, the coordinates of the obstacles should be noted **clockwise** starting from right above the obstacle. As stated in "Detemine the physical field" an object cannot be placed in the origin of placement and should be close to the main branch
4. Make the scout drive your created map in GUI. 
> **Note:** Empty battery case: If during a mapping process the battery gets empthy, the battery can be easily replaced by a new one and the mapping will be proceeded. The scout will initially drive back to the origin of placement and then continue the mapping process.

> **Emergency button:** If the scout must be manually stopped right away, one can immediately shut down the scout using the red emergency button on the scout scout.

5. When the Scout has finishes mapping, the data from the Scout can be downloaded. Since, the scout has two sensors (left and right) the data is converted on the GUI into one map through an algoritm into an amf file.

## 2.7 Map definitions

In the table below you can see which color light the sensors of the Scout will emit during a startup. 

![scout](images/_B3A2767.jpg)

| Sensor State | Color |
|--------------|-------|
| Startup | White / Yellow  |
| Idle | Green-Pink (blink) |
| Mapping | Blue-Pink |
| Drift Correction | White | 


![scout](images/_B3A2752.jpg)


## 2.4 Open the Scout's documentation
Remember that you can always find the documentation of the Scout you are using, by opening up a webpage and enter _scout-ip:5555_ on the browser.
