# Author: Chenyang Zhang
# email: zhangchenyang24@mails.ucas.ac.cn

FROM vpd:ros-noetic-base-cuda

ENV ROS_DISTRO="noetic"
ENV CERES_VERSION="1.12.0"

# 安装编译所需工具 & 依赖库
RUN apt-get update && apt-get install -y --no-install-recommends \
    cmake \
    git \
    build-essential \
    software-properties-common \
    libatlas-base-dev \
    libeigen3-dev \
    libpcl-dev \
    libgoogle-glog-dev \
    libsuitesparse-dev \
    libglew-dev \
    libboost-all-dev \
    libtbb-dev 

# 安装运行时 ROS 相关包
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-${ROS_DISTRO}-cv-bridge \
    ros-${ROS_DISTRO}-tf \
    ros-${ROS_DISTRO}-pcl-ros \
    ros-${ROS_DISTRO}-navigation \
    ros-${ROS_DISTRO}-robot-localization \
    ros-${ROS_DISTRO}-robot-state-publisher \
    ros-${ROS_DISTRO}-image-transport \
    ros-${ROS_DISTRO}-image-transport-plugins \
    ros-${ROS_DISTRO}-rviz \
    ros-${ROS_DISTRO}-xacro \
    libopencv-dev=4.2.0+dfsg-5 \
    && rm -rf /var/lib/apt/lists/*

# 编译安装 Ceres Solver
WORKDIR /home/thirdParty
RUN git clone https://github.com/ceres-solver/ceres-solver.git && \
    cd ceres-solver && \
    git checkout tags/${CERES_VERSION} && \
    mkdir build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release .. && \
    make -j"$(nproc)" && \
    make install && \
    cd / && rm -rf /home/thirdParty/ceres-solver

# 加载 ROS 环境脚本
COPY ./ros_entrypoint.sh /ros_entrypoint.sh
RUN chmod +x /ros_entrypoint.sh

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]