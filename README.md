# hospital-robot-fleet-integration
Open-RMF based 6-floor hospital digital twin featuring traffic-editor generated building maps, fleet navigation infrastructure, lift integration, waypoint systems, and autonomous healthcare logistics simulation in ROS 2 and Gazebo.

https://youtu.be/TUY37A9My_M


# Install ROS 2 Humble

sudo apt update && sudo apt upgrade -y

sudo apt install software-properties-common -y

sudo add-apt-repository universe

sudo apt update && sudo apt install curl -y

sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
-o /usr/share/keyrings/ros-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) \
signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
http://packages.ros.org/ros2/ubuntu \
$(. /etc/os-release && echo $UBUNTU_CODENAME) main" | \
sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

sudo apt update

sudo apt install ros-humble-desktop -y

# Source ROS 2

source /opt/ros/humble/setup.bash

# Install Development Tools

sudo apt update

sudo apt install python3-colcon-common-extensions python3-rosdep \
python3-vcstool git wget -y

Initialize rosdep:
sudo rosdep init

rosdep update

# Create RMF Workspace
mkdir -p ~/rmf_ws/src

cd ~/rmf_ws/src

# Clone Open-RMF Repositories

git clone https://github.com/open-rmf/rmf.git

git clone https://github.com/open-rmf/rmf_demos.git

git clone https://github.com/open-rmf/rmf_traffic_editor.git


# Install RMF Dependencies

cd ~/rmf_ws

rosdep install --from-paths src --ignore-src -r -y

Traffic Editor additionally requires:
sudo apt install python3-shapely python3-yaml python3-requests -y
These dependencies are required by rmf_building_map_tools.

# Build Workspace

cd ~/rmf_ws

source /opt/ros/humble/setup.bash

colcon build --symlink-install

# Source RMF Workspace

source ~/rmf_ws/install/setup.bash

# Launch the world

ros2 launch rmf_demos_gz clinic.launch.xml

ros2 launch rmf_demos_gz_clinic hospital.xml

# Robot Navigation

ros2 run rmf_demos_tasks dispatch_go_to_place \
  -F <fleet_name> \
  -R <robot_name> \
  -p <destination_waypoint> \
  --use_sim_time

  Example:

  ros2 run rmf_demos_tasks dispatch_go_to_place \
  -F tinyRobot \
  -R tinyRobot2 \
  -p coe \
  --use_sim_time


Note: The above launch files you must add inside rmf_ws after clone the workspace add launch file inside src/rmf_demo_gz and src/rmf_demo_gz_classic and install/rmf_demo_gz and install/rmf_demo_gz_ classic then particullarly add these install_launch_clinic.launch.xml file inside install/rmf_demo/share/launch/
