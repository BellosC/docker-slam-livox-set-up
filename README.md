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

>Now the visualisation should start.


Step 4: **Open a third terminal** and save a new ".bag", this time containing the topic "**/laser_cloud_surround**" and give a new name. So, simultaneusly with the other terminals, run this command:

```sh
rosbag record /laser_cloud_surround -O my_new_bag_file.bag
```
> Now you have a new ".bag" file with the elements that you need in order to build your map.


Step5: **Open a fourth terminal** in order to convert your new ".bag" file in to .pcd format. Simply run the command:

```sh
rosrun pcl_ros bag_to_pcd my_new_bag_file.bag /laser_cloud_surround /home/konstantinos/Desktop/
```
>You will find your .pcd files at your chosen path. You can open a terminal, type 
```sh
pcl_viewer <path_of_your_pcd_files>
```
>and see the visualisation of a .pcd file.

>If you want to see the complete model, simply select them all, write *"pcl_viewer"* at your terminal and then drug and drop all .pcd files together.

