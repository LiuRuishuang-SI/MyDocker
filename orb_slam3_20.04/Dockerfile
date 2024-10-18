FROM ubuntu:20.04

LABEL maintainer="LiuRuishuang <https://github.com/LiuRuishuang-SI/>"

# Set environment variables for non-interactive installation
ENV DEBIAN_FRONTEND=noninteractive

# Update and install dependencies
RUN apt update && apt install -y \
        vim \
        zip unzip wget git \
        cmake \
        libeigen3-dev \
        libboost-dev \
        libglew-dev \
        libssl-dev \
        libboost-all-dev \
        libgtk2.0-dev \
        curl \
        gnupg2 \
        lsb-release \
        build-essential

# Install ROS Noetic
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' && \
    curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add -
RUN apt update && apt install -y \
    ros-noetic-desktop \
    python3-rosdep

# Initialize rosdep
RUN rosdep init && \
    rosdep update

# Setup environment
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
SHELL ["/bin/bash", "-c"]
RUN source /opt/ros/noetic/setup.bash

# Install ROS dependencies for building packages
RUN apt install -y python3-rosinstall python3-rosinstall-generator python3-wstool python3-catkin-tools build-essential

# Install ROS dependencies for orb_slam3
RUN apt install -y ros-noetic-hector-trajectory-server mesa-utils libgl1-mesa-dri libgtk-3-dev

# Set the number of threads for parallel builds
ARG NUM_THREADS=2

# OpenCV
ARG OPENCV_VERSION=4.4.0
WORKDIR /tmp
RUN set -x && \
  wget -q https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip && \
  unzip -q ${OPENCV_VERSION}.zip && \
  rm -rf ${OPENCV_VERSION}.zip && \
  cd opencv-${OPENCV_VERSION} && \
  mkdir -p build && \
  cd build && \
  cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DBUILD_DOCS=OFF \
    -DBUILD_EXAMPLES=OFF \
    -DBUILD_JASPER=OFF \
    -DBUILD_OPENEXR=OFF \
    -DBUILD_PERF_TESTS=OFF \
    -DBUILD_TESTS=OFF \
    -DBUILD_opencv_apps=OFF \
    -DBUILD_opencv_dnn=OFF \
    -DBUILD_opencv_ml=OFF \
    -DBUILD_opencv_python_bindings_generator=OFF \
    -DENABLE_CXX11=ON \
    -DENABLE_FAST_MATH=ON \
    -DWITH_EIGEN=ON \
    -DWITH_FFMPEG=ON \
    -DWITH_OPENMP=ON \
    -DWITH_GTK=ON \
    -DWITH_GTK_2_X=OFF \
    .. && \
  make -j${NUM_THREADS} && \
  make install && \
  ldconfig && \
  cd /tmp && \
  rm -rf *

# Pangolin
ARG PANGOLIN_COMMIT=ad8b5f83222291c51b4800d5a5873b0e90a0cf81
WORKDIR /tmp
RUN set -x && \
  git clone https://github.com/stevenlovegrove/Pangolin.git && \
  cd Pangolin && \
  git checkout ${PANGOLIN_COMMIT} && \
  sed -i -e "193,198d" ./src/utils/file_utils.cpp && \
  mkdir -p build && \
  cd build && \
  cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DBUILD_EXAMPLES=OFF \
    -DBUILD_PANGOLIN_DEPTHSENSE=OFF \
    -DBUILD_PANGOLIN_FFMPEG=OFF \
    -DBUILD_PANGOLIN_LIBDC1394=OFF \
    -DBUILD_PANGOLIN_LIBJPEG=OFF \
    -DBUILD_PANGOLIN_LIBOPENEXR=OFF \
    -DBUILD_PANGOLIN_LIBPNG=OFF \
    -DBUILD_PANGOLIN_LIBREALSENSE=OFF \
    -DBUILD_PANGOLIN_LIBREALSENSE2=OFF \
    -DBUILD_PANGOLIN_LIBTIFF=OFF \
    -DBUILD_PANGOLIN_LIBUVC=OFF \
    -DBUILD_PANGOLIN_LZ4=OFF \
    -DBUILD_PANGOLIN_OPENNI=OFF \
    -DBUILD_PANGOLIN_OPENNI2=OFF \
    -DBUILD_PANGOLIN_PLEORA=OFF \
    -DBUILD_PANGOLIN_PYTHON=OFF \
    -DBUILD_PANGOLIN_TELICAM=OFF \
    -DBUILD_PANGOLIN_TOON=OFF \
    -DBUILD_PANGOLIN_UVC_MEDIAFOUNDATION=OFF \
    -DBUILD_PANGOLIN_V4L=OFF \
    -DBUILD_PANGOLIN_VIDEO=OFF \
    -DBUILD_PANGOLIN_ZSTD=OFF \
    -DBUILD_PYPANGOLIN_MODULE=OFF \
    .. && \
  make -j${NUM_THREADS} && \
  make install && \
  cd /tmp && \
  rm -rf *

# ORB-SLAM3
RUN git clone https://github.com/LiuRuishuang-SI/ORB_SLAM3.git ORB_SLAM3 && \
  cd ORB_SLAM3 && \
  ./build.sh

# orb_slam3_ros_wrapper
RUN mkdir -p catkin_ws/src && \
    cd catkin_ws/src && \
    git clone https://github.com/thien94/orb_slam3_ros_wrapper.git && \
    sed -i 's|$ENV{HOME}/Packages/ORB_SLAM3|/tmp/ORB_SLAM3|' orb_slam3_ros_wrapper/CMakeLists.txt && \
    cp /tmp/ORB_SLAM3/Vocabulary/ORBvoc.txt /tmp/catkin_ws/src/orb_slam3_ros_wrapper/config/

WORKDIR /tmp/catkin_ws
RUN /bin/bash -c "source /opt/ros/noetic/setup.bash && catkin build && source devel/setup.bash"