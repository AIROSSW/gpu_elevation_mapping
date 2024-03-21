

# 1. Environment 
## 1.1 Docker Download and Play
sudo docker run --name gpu-elevation-map-airossw -it --privileged \
--env="DISPLAY=0:0" -v=/tmp/.X11-unix:/tmp/.X11-unix --device /dev/bus/usb \
-v=/dev:/dev -w=/home --gpus all \
--network=host elevation_mapping_cupy

## 1.2 Install
### 1.2.1 Linux Application 
sudo apt-get install vim 
sudo apt-get install net-tools

### 1.2.2 Python Library 
cd /home/catkin_ws/src/elevation_mapping_cupy
pip3 install -r requirements.txt
pip3 install cupy-cuda11x==11.6.0 추가
pip3 install numpy == 1.22.4
pip3 install torch 


## 1.3 After Making Image
sudo docker run --name gpu-elevation-map-airossw -it --privileged \
--env="DISPLAY=0:0" -v=/tmp/.X11-unix:/tmp/.X11-unix --device /dev/bus/usb \
-v=/dev:/dev -w=/home \
--network=host [내 이미지 이름]


## 1.4 Other termial 
sudo xhost +local:docker  
sudo docker exec -it 23aff81d93f7  /bin/bash


# 2. Download the code 
mkdir -p /home/catkin_ws/src/Elevation_ws/src
cd /home/catkin_ws/src/Elevation_ws/src
git clone https://github.com/leggedrobotics/elevation_mapping_cupy.git
cd /home/catkin_ws/src/Elevation_ws


## 3 Sourcing And build  
-> if you set 0:0 in the host, set 0 in the local 
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
echo "export DISPLAY=:0" >> ~/.bashrc
source ~/.bashrc

catkin build elevation_mapping_cupy
catkin build convex_plane_decomposition_ros  # If you want to use plane segmentation
catkin build semantic_sensor  # If you want to use semantic sensors



# 4. Excute
## 4.1 Basic Excute 
source devel/setup.bash
export TURTLEBOT3_MODEL=waffle
roslaunch elevation_mapping_cupy turtlesim_simple_example.launch

export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch

## 4.2 Other Excute
## Semantic Pointcloud
export TURTLEBOT3_MODEL=waffle
roslaunch elevation_mapping_cupy turtlesim_semantic_pointcloud_example.launch

## Semantic Image 
export TURTLEBOT3_MODEL=waffle
roslaunch elevation_mapping_cupy turtlesim_semantic_image_example.launch

