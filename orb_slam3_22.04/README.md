DockerFile of ORB-SLAM3
=======================

### Environment

* Ubuntu 22.04


### Build the image

```
sudo docker build -t orb-slam3-22.04 .
```

### Usage

1. Set permission for x server

   ```
   xhost +
   ```

2. Run image

   ```
   sudo docker run --name orb-slam3-22.04 -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix/:/tmp/.X11-unix --privileged --volume=/dev:/dev -v PATH_TO_DATASET:/mnt/share/dataset orb-slam3-22.04 /bin/bash
   ```

   If container is not started

   ```
   sudo docker start orb-slam3-22.04
   sudo docker exec -it orb-slam3-22.04 /bin/bash
   ```

   If multiple terminals are necessary (in another terminal)

   ```
   sudo docker exec -it orb-slam3-22.04 /bin/bash
   ```

3. Run demo

   ```
   cd ORB_SLAM3
   ```

   Launch MH01 with Stereo sensor

   ```
   ./Examples/Stereo/stereo_euroc ./Vocabulary/ORBvoc.txt ./Examples/Stereo/EuRoC.yaml /mnt/share/dataset/MH_01_easy ./Examples/Stereo/EuRoC_TimeStamps/MH01.txt dataset-MH01_stereo
   ```

   Launch MH01 with Stereo-Inertial sensor

   ```
   ./Examples/Stereo-Inertial/stereo_inertial_euroc ./Vocabulary/ORBvoc.txt ./Examples/Stereo-Inertial/EuRoC.yaml /mnt/share/dataset/MH_01_easy ./Examples/Stereo-Inertial/EuRoC_TimeStamps/MH01.txt dataset-MH01_stereoi
   ```
