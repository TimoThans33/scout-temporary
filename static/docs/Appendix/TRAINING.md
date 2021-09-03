# 3. Training - Getting familiar with GUI

This training will discuss a few cases about the scout using the GUI interface. The GUI interface is an online interface where a map can be created for the scout using corresponding coordinates of the location with existing obstacles. With this map GUI can automatically generate a plan for the scout to drive and at the same time collect data of the environment for the robins to know where they are. 

![scout](images/_B3A2758.jpg)

## 3.1 Basic map
The most basic map is a simple rectangle type of field .
1. Determine origin of this map.
> as a rule of thumb take positive y to be the longest side.
2. Measure width and height of the field.
3. Place scout on origin facing towards the positive y-axis in GUI.
4. Create a rectangle in GUI using the measured dimensions.
5. Make the scout drive your map. 

## 3.2 Obstacles
1. In a map there are almost always obstables, which need to be taken into account. Therefore create an obstacle of 1m x 2m right above the scout.
2. Now, place the obstacle in front of the origin of placement and conclude what happens.
3. Solve the problem if the obstacle is placed in the origin of placement.

## 3.3 Mapping progress: Empty battery
If during a mapping process the battery gets empthy, the battery can be easily replaced by a new one and the mapping will be proceeded. The scout will initially drive back to the origin of placement and then continue the mapping process.

## 3.4 Emergency button
If the scout must be manually stopped right away, one can immediately shut down the scout using the red emergency button on the scout scout.

## 3.4 Download map
For the robins to know exactly where they are, the data of the scout is needed to be obtained. Since, the scout has two sensors (left and right) the data is converted on the GUI into one map through an algoritm into an amf file.


## 3.5 Solutions
### 3.5.1 Basic map
1. (0,0,0)
2. 10m X 10m, with clockwise coordinates: [5,-5], [5,-5], [-5,-5], [-5,5]
3. This can be done by clicking on the scout with the right mouse button and change the origin into: x = 0, y = 0, angle = 1.57 rad (since 1,57 = 0.5pi)). It is important to **use dots** and not comma's!
4. Always use **clockwise** coordinates. So start right above the rectangle and work the way down.
5. Click generate grid map, then click generate plan, press play one time to initialize the scout and press another time on play to start the scout.

### 3.5.2 Obstacles
1. As for the map, also the coordinates of the obstacles should be **clockwise** starting from right above the obstacle.
2. The object cannot be placed in the origin of placement.
3. The origin of placement should be moved and should not interfere with any obstacle. 
