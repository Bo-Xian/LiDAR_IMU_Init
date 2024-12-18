# Author: Tae Young Kim
# email: tyoung96@yonsei.ac.kr

FROM ros:melodic

# Install PCL & Eigen & essential libraries
RUN set -eux && \
    apt-get update && \
    apt-get install -y  cmake \
                        libeigen3-dev \
                        libpcl-dev \
                        ros-$ROS_DISTRO-rviz \
                        ros-$ROS_DISTRO-pcl-ros \
                        ros-$ROS_DISTRO-eigen-conversions \
                        libatlas-base-dev \
                        libgoogle-glog-dev \
                        libsuitesparse-dev \
                        libglew-dev \
                        wget

# Install matplotlib
RUN apt-get install -y python-pip python-tk && pip install matplotlib

# Install ceres-solver
WORKDIR /home/
# RUN wget https://github.com/ceres-solver/ceres-solver/archive/refs/tags/2.0.0.tar.gz && \
#     tar zxf 2.0.0.tar.gz && \
#     rm -rf 2.0.0.tar.gz && \
#     cd ceres-solver-2.0.0 && \
#     mkdir build && cd build && \
#     ls && \
#     cmake -DCMAKE_BUILD_TYPE=Release ./ceres-solver-2.0.0 && \
#     make -j2 && \
#     make install

RUN wget https://github.com/ceres-solver/ceres-solver/archive/refs/tags/2.0.0.tar.gz && \
    ls -l && \
    tar zxf 2.0.0.tar.gz && \
    rm -rf 2.0.0.tar.gz && \
    ls -l ceres-solver-2.0.0 && \
    cd ceres-solver-2.0.0 && \
    mkdir build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release .. && \
    make -j$(nproc) && \
    make install && \
    cd ../.. && \
    rm -rf ceres-solver-2.0.0

# Install livox driver
WORKDIR /home/catkin_ws/src
RUN wget https://github.com/Livox-SDK/livox_ros_driver/archive/refs/tags/v2.6.0.tar.gz && \
    tar zxf v2.6.0.tar.gz && \
    rm -rf v2.6.0.tar.gz

# Copy source code and catkin_make workspace
# COPY LiDAR_IMU_Init /home/catkin_ws/src/LiDAR_IMU_Init
# RUN /bin/bash -c '. /opt/ros/$ROS_DISTRO/setup.bash; catkin_init_workspace; cd .. && catkin_make'

# Load ROS environment at each run
COPY LiDAR_IMU_Init/docker/ros_entrypoint.sh /
RUN chmod 755 /ros_entrypoint.sh
ENTRYPOINT ["/ros_entrypoint.sh"]

CMD ["bash"]
