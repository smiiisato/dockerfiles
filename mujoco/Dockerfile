FROM nvidia/cudagl:11.3.1-devel-ubuntu20.04

LABEL maintainer="smiiisato<m-sonoda@jsk.imi.i.u-tokyo.ac.jp>"

# Ubuntu 20.04 image with
# - CUDA 11.3.1
# - ROS Noetic
# - Mujoco 2.0
# - Python 3.8
# - Pytorch 1.9.0

ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /mujoco_ws

RUN apt-get update \
    && apt-get dist-upgrade -q -y \
    && rm -rf /var/lib/apt/lists/*

# Install ROS Noetic
RUN apt-get update \
    && apt-get install -q -y \
    curl \
    gnupg2 \
    lsb-release \
    && curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - \
    && sh -c 'echo "deb http://packages.ros.org/ros/ubuntu focal main" > /etc/apt/sources.list.d/ros-latest.list' \
    && apt-get update \
    && apt-get install -q -y \
    ros-noetic-desktop-full \
    && rm -rf /var/lib/apt/lists/*
# setup keys
RUN curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - 
# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu focal main" > /etc/apt/sources.list.d/ros1-latest.list
# install bootstrap tools
RUN apt-get update && apt-get install --no-install-recommends -y \
    python3-rosdep \
    python3-rosinstall \
    python3-vcstools \
    python3-catkin-tools \
    python3-wstool \
    && rm -rf /var/lib/apt/lists/*

# setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
# bootstrap rosdep
RUN rosdep init
# install ros packages
RUN apt-get update && apt-get install -y \
    ros-noetic-desktop-full \
    && rm -rf /var/lib/apt/lists/*

# -------------------------------------------------------
# Basic Configuration
# -------------------------------------------------------
RUN apt-get update -q && \
    apt-get install -q -y \
        acpi \
        apt \
        apt-cacher-ng \
        apt-utils \
        aptitude \
        bc \
        ccache \
        cmake \
        cmake-curses-gui \
        cron \
        curl \
        emacs \
        git \
        hddtemp \
        htop \
        indicator-multiload \
        ipython3 \
        less \
        lsb-release \
        man-db \
        python3.8 \
        python3-pip \
        python3-setuptools \
        software-properties-common \
        ssh \
        sudo \
        wget \
        xdg-user-dirs-gtk \
        vim \
        emacs \
        tmux \
        git \
    && rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/*

# -------------------------------------------------------
# PyTorch Installation
# -------------------------------------------------------
RUN pip3 install torch==2.0.1+cu117 -f https://download.pytorch.org/whl/torch_stable.html

# -------------------------------------------------------
# Mujoco Installation
# -------------------------------------------------------
RUN pip3 install mujoco==2.3.4

# -------------------------------------------------------
# Install other python packages
# -------------------------------------------------------
RUN pip3 install gymnasium==0.28.1
RUN pip3 install numpy==1.23.5
RUN pip3 install rospkg==1.5.0
RUN pip3 install pandas==2.0.2
RUN pip3 install argparse==1.1

# -------------------------------------------------------
# Install stable-baselines3
# -------------------------------------------------------
RUN pip3 install stable-baselines3==2.1.0

# -------------------------------------------------------
# Install tensorboard
# -------------------------------------------------------
RUN pip3 install tensorboard==2.13.0

# -------------------------------------------------------
# bashrc
# -------------------------------------------------------
RUN echo '' >> ~/.bashrc
RUN echo '# CUDA' >> ~/.bashrc
RUN echo 'export PATH=/usr/local/cuda/bin:${PATH}' >> ~/.bashrc
RUN echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:${LD_LIBRARY_PATH}' >> ~/.bashrc
RUN echo '' >> ~/.bashrc
RUN echo '# ROS' >> ~/.bashrc
RUN echo 'source /opt/ros/noetic/setup.bash' >> ~/.bashrc
RUN echo '' >> ~/.bashrc
RUN echo '# Mujoco' >> ~/.bashrc
RUN echo 'source /mujoco_ws/devel/setup.bash' >> ~/.bashrc
RUN echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/nvidia' >> ~/.bashrc
RUN echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/leus/.mujoco/mujoco210/bin' >> ~/.bashrc

