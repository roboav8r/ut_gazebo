# ut_gazebo

## Prerequisites:
Assumes Gazebo and ROS are already installed.

## Installation:
Create a ut_meshes model folder in Gazebo:
```mkdir -p ~/.gazebo/models/ut_meshes```

Copy the three .dae files from UTBox into the ut_meshes folder.

Clone the ut_gazebo repository into your catkin workspace and build:
```
cd ~/catkin_ws/src && git clone https://github.com/roboav8r/ut_gazebo.git
cd ~/catkin_ws && catkin build
source devel/setup.bash
```
## Usage:

    Once the package is installed and built and the workspace is sourced, simply launch the Gazebo .world you wish to use:
        roslaunch ut_gazebo ut_class_mesh.launch
        roslaunch ut_gazebo ut_class_mesh_realistic.launch
        roslaunch ut_gazebo ut_class_mesh_notrees.launch
    To spawn a robot on Speedway just east of EPS, you can use the put_robot_in_world option and the example launch script as a starter:
        roslaunch ut_gazebo ut_class_mesh.launch put_robot_in_world:=true put_robot_in_world_package:=ut_gazebo put_robot_in_world_launch:=put_robot_in_world_example.launch
        Note that this won't work unless you adapt the put_robot_in_world_example to use your robot's description, name, and controller. The included file is set up for our Walrus robot.
    Getting terrain classification data (general process, not tested or implemented yet)
        Download the JSON data arrays from UTBox.
        In a separate script, load the pandas dataframe using pandas.read_json('/path/to/UTCampus_class_array_373_255.json')
        Get your robot odometry in the mesh frame. There is a static transform between mesh and world frames provided by the launch file.
        e.g., (0,0) in the world frame is (373,255) in the mesh frame.
        Send a query to the JSON database using the current location in the mesh frame. This uses the iloc command, per the .boxnote in UTBox. The result of the query should be the terrain type.

## Miscellaneous:

    UTCampus_Class_Mesh.dae and UTCampus_Class_Mesh_realistic.dae contain the same mesh and segmentation data, but are colored differently.
    UTCampus_Class_noTrees.dae is the same as UTCampus_Class_Mesh.dae, only without the foliage.
    The maps are aligned such that +X = East, +Y = North, and +Z = Up. There is a slight offset between the straight segments of roads and cardinal directions, just like the actual UT campus.
    To spawn the robot at a different location, change:
        The static_transform_publisher line, x/y/z position in the .launch file (e.g. args="143 325 24.5 ... ")
        The pose line in the associated .world file. The sign of the pose line should be the opposite of the static transform publisher x/y/z position (e.g. <pose>-143 -325 -24.5 ...)
        File and model names as desired.
        For a comparative example, see ut_class_mesh and ut_class_mesh_tower, which spawn the mesh at speedway and the base of UT tower respectively
