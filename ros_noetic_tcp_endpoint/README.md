DockerFile of ROS-TCP-Endpoint for ROS 1 Noetic
=======================

### Build the image

```bash
sudo docker build -t ros-noetic-tcp-endpoint .
```

## Usage

### Run the Docker Container

1. Set permission for x server

    ```bash
    xhost +
    ```

2. Run the Docker container from the image you just built. Map the port that the ROS TCP Endpoint will use. In this example, we'll use port `10000`:

    ```bash
    docker run -it --rm --name ros-noetic-tcp-endpoint \
        -p 10000:10000 ros-humble-tcp-endpoint \
        -e DISPLAY=$DISPLAY \
        -v /tmp/.X11-unix/:/tmp/.X11-unix
    ```

### Access the Running Container

If you need to access the running container, open a new terminal and execute:

```bash
docker exec -it ros-noetic-tcp-endpoint /bin/bash
```

### Start the ROS TCP Endpoint Node

Inside the Docker container, start the ROS TCP Endpoint node to allow Unity to communicate with ROS:

```bash
roslaunch ros_tcp_endpoint endpoint.launch tcp_port:=10000
```

