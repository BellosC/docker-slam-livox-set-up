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

Now the visualisation should start.
