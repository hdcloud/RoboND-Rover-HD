
[//]: # (Image References)

[image1]: ./robo_sim_notebook.png
[image2]: ./robo_sim_auto.png


---
### Writeup / README


#### 1.Rover Project Test Notebook
I have run the functions provided in the notebook on test images, and verified that video is played and the video output is generated.

In the process_image(), the following procedures were added. 

    1) Define source and destination points for perspective transform
    2) Apply perspective transform
    3) Apply color threshold to identify navigable terrain/obstacles/rock samples
    4) Convert thresholded image pixel values to rover-centric coords
    5) Convert rover-centric pixel values to world coords
    6) Update worldmap 

From this, worldmap and warped images could be displayed on the video.

Obstacle is the area where 'warped' image has a value other than 0 which means not black, and the threshed image is excluded.
    
    mask_map = warped[:,:,] != [0,0,0] 
    threshed_map = threshed == 1
    mask_map[threshed_map, 0] = 0
    obs_map = mask_map[:,:,0]

Rock can be identified by checking the RGB values against its threshold (120, 120, 40) which has been selected by checking the RGB value of the sample rock image.

    rock_level = (120, 120, 40)
    rock_map = ((warped[:,:,0] > rock_level[0]) & \
                (warped[:,:,1] > rock_level[1]) & \
                (warped[:,:,2] < rock_level[2]))
    
![Video Image][image1]


### Autonomous Navigation and Mapping

#### 1. percepton_step()
The same procedures done in the notebook are applied in the perception_step().
The color threshed images, the obstacle images and the rock images were stored into the vision_image in Rover's state object, which will be displayed as the color threshed images with differnt colors.

After converting the color threshed images (Navigable, Obstacles and Rocks) into the Rover centric coordinates, and then converted into the world coordinates using pix_to_world(). These images (navigable, obstacle and Rock) are projected onto the world map.

After the navigable areas (xpos, ypos) are converted into a rover centric coordinates, they are also used to calculate the distance and angle to each pixel in the area, so that the Rover can find the best direction from the mean angle.

#### 2. Rover running in the autonomous mode and improvements to be made  


The simulation setting is as follows.
* Screen resoluton: 800 x 600
* Graphics quality: Good
* FPS: 30 ~ 40

Regarding the decision rulte, no change has been made, since the Rover at least worked somewhat ok with the Rover.nav_dists, Rover.nav_angles calculated using 'to_polar_coords()' with the input of rover centric coordinates of the navigable image.

To make this project more complete, I would like to work more on the determination of whether all the golds are collected, and then method/algorithm of how to make the rover return back to the original location. 

Another thing to improve is the worldmap view is repeatedly drawn with the current worldmap images with navigable, obstacle and rock, and thus it would be hard to see the current rover's location on the worldmap. For this, I would like to further improve the worldmap view be displayed with the current state of the worldmap.

This project has been challenging due to the lack of understanding of the topic and lack of time. However, I've found it really exciting to work on this Robotics topic. 
Hopefully, I would like to revisit this project and improve greatly and also have more understanding of the whole content of this topic.

![Auto Navi][image2]


