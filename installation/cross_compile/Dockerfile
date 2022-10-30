FROM ubuntu:20.04

# Install dependencies


ENV TZ=Europe/Madrid
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt update
RUN apt install -y tzdata
RUN apt-get update && apt-get install -y \
    wget \
    tar \
    git \
    cmake \
    qemu-user-static \
    python3-pip \
    python3-numpy \
    sshfs \
    rsync \ 
    nano \
    vim \
    curl \
    software-properties-common \
    && rm -rf /var/lib/apt/lists/*

RUN echo user_allow_other >> /etc/fuse.conf
# Add user
RUN useradd -m develop
RUN echo "develop:develop" | chpasswd
RUN usermod -aG sudo develop
RUN apt-get update && apt-get install -y \
    python3.9 \
    python3.9-dev \
    python3.9-distutils \
    python3-pip

# ROS2 developmnet dependencies
USER develop
RUN pip3 install --no-cache-dir \
    rosinstall_generator \
    vcstool \
    lark-parser
ENV PATH=/home/develop/.local/bin/:$PATH

# Install compiler
USER root
WORKDIR /tmp
RUN wget https://github.com/Pro/raspi-toolchain/releases/latest/download/raspi-toolchain.tar.gz \
    && tar xfz raspi-toolchain.tar.gz --strip-components=1 -C /opt \
    && rm raspi-toolchain.tar.gz
RUN rm /usr/bin/python3
RUN ln -s /usr/bin/python3.9 /usr/bin/python3
RUN ln -s /usr/bin/python3.9 /usr/bin/python

USER develop
RUN pip3 install --no-cache-dir \
    numpy==1.19.5 \
    colcon_core \
    colcon_ros \
    colcon-common-extensions \
    lark

# Prepare workspace
USER develop
WORKDIR /home/develop
COPY toolchain.cmake toolchain.cmake
COPY bashrc.sh bashrc.sh
RUN cat bashrc.sh >> $HOME/.bashrc
WORKDIR /home/develop/ros2_ws
