# docker-slam-livox-set-up

Step1: Make account to docker and downlad docker (https://docs.docker.com/get-docker/)

Step2: Download horizon_highway_slam package from Github:

```sh
git clone https://github.com/Livox-SDK/horizon_highway_slam.git
```

```sh
cd horizon_highway_slam/
```

```sh
sudo docker build -t horizon_highway_slam .
```

> the 1st and 3rd command you run them only the first time that you do it. Next times you can skip those 2 commands.

> While you are doing this procedure, **create a folder called "shared_dir" at home directory**. Docker and Host can share files through this folder (for example a rosbag file).Now go back to terminal and continue with the next command:


```sh
sudo ./run.sh
```

>Afer successfully **entering the docker** container, you can directly launch the horizon_highway_slam:

```sh
root@HOSTNAME:/# roslaunch horizon_highway_slam horizon_highway_slam.launch BagName:=YouTube_highway_demo.bag IMU:=2
```
> **Change** the BagName variable and put your ".bag" file. Of course, as said before, this ".bag" file must also be placed inside the "shared_dir" folder that you created.

Step 3: **Open a second terminal** and run your bagfile using the rviz of the host machine (your computer). 

```sh
rosrun rviz rviz -d horizon_highway_slam/rviz_cfg/horizon_highway_slam.rviz
```


> Now the visualisation should start.


Step 4: **Open a third terminal** and save a new ".bag", this time containing the topic "**/laser_cloud_surround**" and give a new name. So, simultaneusly with the other terminals, run this command:

```sh
rosbag record /laser_cloud_surround -O my_new_bag_file.bag
```
> Now you have a new ".bag" file with the elements that you need in order to build your map.


Step 5: **Open a fourth terminal** in order to convert your new ".bag" file in to .pcd format. Simply run the command:

```sh
rosrun pcl_ros bag_to_pcd my_new_bag_file.bag /laser_cloud_surround /home/konstantinos/Desktop/
```
>You will find your .pcd files at your chosen path. You can open a terminal, type 
```sh
pcl_viewer <path_of_your_pcd_files>
```
> and see the visualisation of a .pcd file.

> if you want to see the **complete model**, simply select them all, write *"pcl_viewer"* at your terminal and then drug and drop **all .pcd files together**.

![Screenshot from 2021-05-26 12-02-09](https://user-images.githubusercontent.com/70270581/119633405-62be5500-be1a-11eb-8ac2-bc086b8e312c.png)

--------------------------------------------------------
--------------------------------------------------------
--------------------------------------------------------
# Create your own rosbag:

Step 1: Connect livox LIDAR

Step 2: Use **livox_ros_driver** package in order to start scanning. So, open **terminal #1** and type:

```sh 
roslaunch livox_ros_driver livox_lidar.launch
```
Step 3:Open **terminal #2** and listen and record all available topics (-a) and also give a name to you ".bag" file (-O) by typing the command:

```sh
rosbag record -a -O name_of_my_bag.bag
```

Step 4: Run both terminal tabs smultaneously.At home folder you will find your bag file.

---------------------------------------------------------
---------------------------------------------------------
---------------------------------------------------------
# Run your own rosbag file through the slam package and create a 3D model:

Step 1: Open **terminal #1** and type:

```sh
cd horizon_highway_slam/
```
```sh
sudo ./run.sh
```
> once you enter docker terminal type:
```sh
roslaunch horizon_highway_slam horizon_highway_slam.launch BagName:=name_of_my_bag.bag IMU:=0
```
> **carefull**, this time we set the parameter **IMU:=0** to zero. I am not sure yet why it works only when i do this but in the future i will figure it out. The instruction of the Horizon_HIghway_slam are these:

> IMU: choose IMU information fusion strategy, there are 3 mode:

> 0 - whithout using IMU information, pure lidar SLAM.

> 1 - using gyroscope integration angle to eliminate the rotation distortion of the lidar point cloud in each frame.

> 2 - tightly coupling IMU and lidar information to improve SLAM effects. Requires a careful initialization process, and still in beta stage.

Step 2: Open **terminal #2** and type:

```sh
rosrun rviz rviz -d horizon_highway_slam/rviz_cfg/horizon_highway_slam.rviz
```
Step 3: Open **terminal #3** and and record the topic **/laser_cloud_surround** and name it (-O):

```sh
rosbag record /laser_cloud_surround -O my_final_bag.bag
```

Step 4: Run all terminals together and when the procedure ends close them. Now you will find your "my_final_bag.bag" file at your home folder.

Step 5: Convert your **.bag file to .pcd file(s)**. In the command you have to type the name of your .bag file, the topic that you want to use in order to create the .pcd file and also the path that your .pcd file(s) will be saved.So, open a new terminal and type:

```sh
rosrun pcl_ros bag_to_pcd my_final_bag.bag /laser_cloud_surround /home/konstantinos/Desktop
```
Step 6: You can open your .pcd files all together with CloudCompare and examine your work or you can open a terminal and type this:

```sh
pcl_viewer path1.pcd, path2.pcd, path3.pcd etc
```
> you can also select all .pcd files with ctrl+A and drug them to terminal after the command *pcl_viewer *

>The final result will be something like this:
>![Screenshot from 2021-05-26 20-10-14](https://user-images.githubusercontent.com/70270581/119703203-fc0f5a80-be5e-11eb-9019-cc47a9f0a184.png)

