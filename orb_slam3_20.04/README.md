DockerFile of ORB-SLAM3
=======================

### Environment

* Ubuntu 20.04
* Ros Noetic


### Build the image

```bash
sudo docker build -t orb-slam3-20.04 .
```

### Usage

1. Set permission for x server

   ```bash
   xhost +
   ```

2. Run image

   ```bash
   sudo docker run --name orb-slam3-20.04 -it \
      -e DISPLAY=$DISPLAY \
      -v /tmp/.X11-unix/:/tmp/.X11-unix \
      --privileged \
      --volume=/dev:/dev \
      -v <PATH_TO_DATASET>:<PATH_TO_DATASET> \
      orb-slam3-20.04 /bin/bash
   ```

   If container is not started

   ```bash
   sudo docker start orb-slam3-20.04
   ```

   If multiple terminals are necessary (in another terminal)

   ```bash
   sudo docker exec -it orb-slam3-20.04 /bin/bash
   ```

3. Run demo

   ```bash
   cd /tmp/ORB_SLAM3
   ```

   To launch the demo using the stereo sensor with the `MH_01_easy` dataset:

   ```bash
   ./Examples/Stereo/stereo_euroc ./Vocabulary/ORBvoc.txt \
      ./Examples/Stereo/EuRoC.yaml \
      <PATH_TO_DATASET>/MH_01_easy \
      ./Examples/Stereo/EuRoC_TimeStamps/MH01.txt \
      dataset-MH01_stereo

   ```

   To launch the demo using the stereo-inertial sensor with the `MH_01_easy` dataset:

   ```bash
   ./Examples/Stereo-Inertial/stereo_inertial_euroc ./Vocabulary/ORBvoc.txt \
      ./Examples/Stereo-Inertial/EuRoC.yaml \
      <PATH_TO_DATASET>/MH_01_easy \
      ./Examples/Stereo-Inertial/EuRoC_TimeStamps/MH01.txt \
      dataset-MH01_stereoi
   ```

3. Run ROS example

   (Optional) If you encounter driver issues, you can switch to the software rasterizer using:

   ```bash
   export LIBGL_ALWAYS_SOFTWARE=1
   ```

   Launch ORB-SLAM3 in ROS with Stereo sensor/Stereo-Inertial sensor:

   ```bash
   roslaunch orb_slam3_ros_wrapper euroc_stereo.launch 
   roslaunch orb_slam3_ros_wrapper euroc_stereoimu.launch
   ```

   Open another terminal and play `MH_01_easy.bag` file:

   ```bash
   rosbag play <PATH_TO_DATASET>/MH_01_easy.bag
   ```
