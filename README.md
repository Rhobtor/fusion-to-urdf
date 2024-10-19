# fusion2urdf ROS2
This is an Add-In script for Fusion360 to export the 3D models to a robot description package which cocntains urdf, .stl, scripts, etc.. to make it executable in ROS2.

## Installation

Run the following command in your shell.

##### Windows (In PowerShell)

```powershell
cd <path to fusion2urdf-ros2>
Copy-Item ".\URDF_Exporter_Ros2\" -Destination "${env:APPDATA}\Autodesk\Autodesk Fusion 360\API\Scripts\" -Recurse
```

##### macOS (In bash or zsh)

```bash
cd <path to fusion2urdf-ros2>
cp -r ./URDF_Exporter_Ros2 "$HOME/Library/Application Support/Autodesk/Autodesk Fusion 360/API/Scripts/"
```

## What is this script?
This is a fusion 360 script to export urdf from fusion 360 directly.

This exports:
* .urdf file of your model
* .launch.py files to simulate your robot on gazebo and rviz
* .stl files of your model


## Before using this script

Before using this script, make sure that your model has all the "links" as components. You have to define the links by creating corresiponding components. For example, this model(https://grabcad.com/library/spotmini-robot-1) is not supported unless you define the "base_link". 

In addition to that, you should be careful when define your joints. The **parent links** should be set as **Component2** when you define the joint, not as Component1. For example, if you define the "base_link" as Component1 when you define the joints, an error saying "KeyError: base_link__1" will show up.

Sometimes this script exports abnormal urdf without any error messages. In that case, the joints should have problems. Redefine the joints and run again.

In addition to that, make sure that this script currently supports only "Rigid", "Slider" and "Revolute".




## How to use

### Install in Shell 

Run the [installation command](#installation) in your shell.

### Run in Fusion 360

Click ADD-INS in fusion 360, then choose ****Scripts and Add-Ins > URDF_Exporter_Ros2****. 

**This script will change your model. So before running it, copy your model to backup.**

Run the script and wait a few seconds(or a few minutes). Then a folder dialog will show up. Choose where you want to save the urdf (A folder "Desktop/test" is chosen in this example").
Maybe some error will occur when you run the script. Fix them according to the instruction. In this case, something wrong with joint "Rev 7". Probably it can be solved by just redefining the joint.

**You must define the base component**. Rename the base component as "base_link". 


Now you can run the script. Let's run the script. Choose the folder to save and wait for a few seconds. You will see many "old_components" in the components field, please ignore them. 

You have successfully exported the urdf file. Also, you got `.stl` files in the "Desktop/test/mm_stl" repository. This will be required at the next step. The existing fusion CAD file is no more needed. You can delete it. 

The folder "Desktop/test" will be required in the next step. Move them into your ros environment.


#### In your ROS environment

Place the generated _description package directory in your own ROS workspace. "model_ws" is used in this example.
Then, run catkin_make in catkin_ws.

```bash
cd ~/model_ws/
colcon build
```

Now you can see your robot in rviz by using the following command.

Open a new terminal

```bash
cd ~/model_ws/
source install/setup.bash
ros2 launch (whatever your robot_name is)_description display.launch.py
```

If you want to simulate your robot on gazebo, just run
```bash
ros2 launch (whatever your robot_name is)_description gazebo.launch.py
```

**Enjoy your Fusion 360 and ROS2 life!**
