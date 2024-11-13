# Use ROS 1 Noetic base image
FROM ros:noetic-ros-base

RUN apt-get update && apt-get install -y \
    git \
    python3 \
    python3-pip \
    python-is-python3 \
    ros-noetic-catkin \
    ros-noetic-rviz \ 
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /home/dev_ws/src

# Clone the ROS TCP Endpoint for ROS 1
RUN git clone https://github.com/Unity-Technologies/ROS-TCP-Endpoint /home/dev_ws/src/ros_tcp_endpoint

RUN echo "cd /home/dev_ws" >> ~/.bashrc

# Set up and build the catkin workspace
RUN /bin/bash -c "source /opt/ros/noetic/setup.bash && cd /home/dev_ws && catkin_make"

# Source ROS and workspace setup in bashrc
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
RUN echo "source /home/dev_ws/devel/setup.bash" >> ~/.bashrc

# Expose a port for Unity connection
EXPOSE 10000

CMD ["bash"]
