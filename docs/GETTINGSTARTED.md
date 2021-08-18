# 2. Getting Started
To obtain information about the environment (a map) with the Scout, a couple of things need to be set first:

## 2.1 Determine the physical field

    - Determine the field you would like the Scout to map for you.
    - Check the floor on imperfections and possible rubbish (fix and clean those if needed).
    - Mark the 4 corners (e.g. with tape) and measure the dimensions of your determined field.
    - Determine and mark the origin of this field (obstacles can **never** be on your main branch of your determined field! Try to locate the obstacles close to your main brach instead. By putting the obstacles closer to your main branch, the robot is less likely to bump into an obstacle accidentally).
    - Measure the outlines of the field and the distance perpendicular to the each outline measured from the origin of your field (by this you also determine the values of the field corners). If possible, this can be done with a laser to measure the outlines of the field.
    - Measure the locations of your obstacles in your field.

## 2.2 Turn on the Scout
Before using the Scout, make sure you add full batteries to the battery holders of the Scout and press the <span style="color:green">green</span> power button. In the table below you can see which color light the sensors of the Scout will emit during a startup. 

![scout](images/_B3A2767.jpg)

| Sensor State | Color |
|--------------|-------|
| Startup | White / Yellow  |
| Idle | Green-Pink (blink) |
| Mapping | Blue-Pink |
| Drift Correction | White | 


![scout](images/_B3A2752.jpg)

## 2.3 Connect to WIFI
To make use of the scout, you need wifi. Connect to the correct wifi by connecting to as_demo (if available), the Scout's network or a hotspot on your own laptop.
> **Important note:** The IP adress of your Scout is always: 192.168.8. **Scout No. + 10**

## 2.4 Open the Scout's documentation
Remember that you can always find the documentation of the Scout you are using, by opening up a webpage and enter _scout-ip:5555_ on the browser.

## 2.5 Open GUI & virtualize field
GUI is an online interface where you can control the mapping of your Scout. To go to the GUI, and open up a webpage by entering _scout-ip:8800_ on the browser.

## 2.6 Virtualize field in GUI
GUI is an online interface where you can create a virtual map for the Scout you are using. Within this virtualized map you use the coordinates of the measured outlines, corners and obstacles of your field. With this virtualized map GUI can automatically generate a plan for the Scout to drive & collect data of the environment.  

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
- **Main Branch**: the main y/ north-south axis  
- **Branches**: all x / east-west axis 
- **Fillings**: all in y / north-south axis 

