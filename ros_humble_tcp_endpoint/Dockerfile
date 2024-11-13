# Use ROS 2 Humble base image
FROM ros:humble-ros-base

RUN apt-get update && apt-get install -y \
    ros-humble-rosidl-default-generators \
    ros-humble-rosidl-default-runtime \
    ros-humble-launch \
    ros-humble-launch-ros \
    ros-humble-rviz2 \
    ros-humble-cv-bridge \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /home/dev_ws/src

RUN git clone https://github.com/Unity-Technologies/ROS-TCP-Endpoint /home/dev_ws/src/ros_tcp_endpoint -b ROS2v0.7.0

RUN echo "cd /home/dev_ws" >> ~/.bashrc

RUN /bin/bash -c "source /opt/ros/humble/setup.bash && cd /home/dev_ws && colcon build"

RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
RUN echo "source /home/dev_ws/install/setup.bash" >> ~/.bashrc

# Expose a port for Unity connection
EXPOSE 10000

CMD ["bash"]
