# realsenseros
a modified realsenseros for d455 
 - #### Install [ROS Melodic or later on Windows 10](https://wiki.ros.org/Installation/Windows)


### There are 2 sources to install realsense2_camera from:

* ### Method 1: The ROS distribution:

  *Ubuntu*

    realsense2_camera is available as a debian package of ROS distribution. It can be installed by typing:
    
    ```sudo apt-get install ros-$ROS_DISTRO-realsense2-camera```

    This will install both realsense2_camera and its dependents, including librealsense2 library and matching udev-rules.

    Notice:
    * The version of librealsense2 is almost always behind the one availeable in RealSense&trade; official repository.
    * librealsense2 is not built to use native v4l2 driver but the less stable RS-USB protocol. That is because the last is more general and operational on a larger variety of platforms.
    * realsense2_description is available as a separate debian package of ROS distribution. It includes the 3D-models of the devices and is necessary for running launch files that include these models (i.e. rs_d435_camera_with_model.launch). It can be installed by typing:
    `sudo apt-get install ros-$ROS_DISTRO-realsense2-description`

  *Windows*

    **Chocolatey distribution Coming soon**

* ### Method 2: The RealSense&trade; distribution:
     > This option is demonstrated in the [.travis.yml](https://github.com/intel-ros/realsense/blob/development/.travis.yml) file. It basically summerize the elaborate instructions in the following 2 steps:


   ### Step 1: Install the latest Intel&reg; RealSense&trade; SDK 2.0

    *Ubuntu*
    
    Install librealsense2 debian package:
    * Jetson users - use the [Jetson Installation Guide](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation_jetson.md)
    * Otherwise, install from [Linux Debian Installation Guide](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md#installing-the-packages)
      - In that case treat yourself as a developer. Make sure you follow the instructions to also install librealsense2-dev and librealsense2-dkms packages.

    *Windows* 
    Install using vcpkg

        `vcpkg install realsense2:x64-windows` 

   #### OR
   - #### Build from sources by downloading the latest [Intel&reg; RealSense&trade; SDK 2.0](https://github.com/IntelRealSense/librealsense/releases/tag/v2.50.0) and follow the instructions under [Linux Installation](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md)


   ### Step 2: Install Intel&reg; RealSense&trade; ROS from Sources
   - Create a [catkin](http://wiki.ros.org/catkin#Installing_catkin) workspace
   *Ubuntu*
   ```bash
   mkdir -p ~/catkin_ws/src
   cd ~/catkin_ws/src/
   ```
   *Windows*
   ```batch
   mkdir c:\catkin_ws\src
   cd c:\catkin_ws\src
   ```

   - Clone the latest Intel&reg; RealSense&trade; ROS from [here](https://github.com/intel-ros/realsense/releases) into 'catkin_ws/src/'
   ```bashrc
   git clone https://github.com/IntelRealSense/realsense-ros.git
   cd realsense-ros/
   git checkout `git tag | sort -V | grep -P "^2.\d+\.\d+" | tail -1`
   cd ..
   ```
   - Make sure all dependent packages are installed. You can check .travis.yml file for reference.
   - Specifically, make sure that the ros package *ddynamic_reconfigure* is installed. If *ddynamic_reconfigure* cannot be installed using APT or if you are using *Windows* you may clone it into your workspace 'catkin_ws/src/' from [here](https://github.com/pal-robotics/ddynamic_reconfigure/tree/kinetic-devel)


   ```bash
  catkin_init_workspace
  cd ..
  catkin_make clean
  catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release
  catkin_make install
  ```

  *Ubuntu*
  ```bash
  echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
  source ~/.bashrc
  ```

  *Windows*
  ```batch
  devel\setup.bat
  ```

## Usage Instructions

### Start the camera node
To start the camera node in ROS:

```bash
roslaunch realsense2_camera rs_camera.launch
```

This will stream all camera sensors and publish on the appropriate ROS topics.

Other stream resolutions and frame rates can optionally be provided as parameters to the 'rs_camera.launch' file.

### Published Topics
The published topics differ according to the device and parameters.
After running the above command with D435i attached, the following list of topics will be available (This is a partial list. For full one type `rostopic list`):
- /camera/color/camera_info
- /camera/color/image_raw
- /camera/color/metadata
- /camera/depth/camera_info
- /camera/depth/image_rect_raw
- /camera/depth/metadata
- /camera/extrinsics/depth_to_color
- /camera/extrinsics/depth_to_infra1
- /camera/extrinsics/depth_to_infra2
- /camera/infra1/camera_info
- /camera/infra1/image_rect_raw
- /camera/infra2/camera_info
- /camera/infra2/image_rect_raw
- /camera/gyro/imu_info
- /camera/gyro/metadata
- /camera/gyro/sample
- /camera/accel/imu_info
- /camera/accel/metadata
- /camera/accel/sample
- /diagnostics
