FROM ros:melodic

RUN apt-get update && apt-get install -y --no-install-recommends \
  && apt-get install -y --no-install-recommends \
  wget nano build-essential libomp-dev clang lld\
  ros-melodic-geodesy ros-melodic-pcl-ros ros-melodic-nmea-msgs \
  ros-melodic-rviz ros-melodic-tf-conversions ros-melodic-libg2o \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /root/catkin_ws/src
WORKDIR /root/catkin_ws/
#RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin_init_workspace'
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo 'root:password' | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN echo 'PermitEmptyPasswords yes' >> /etc/ssh/sshd_config

CMD ["/usr/sbin/sshd", "-D"]
RUN apt-get update && apt-get install -y software-properties-common
RUN add-apt-repository -y ppa:borglab/gtsam-release-4.0
RUN apt-get install cmake
RUN apt-get install libgoogle-glog-dev libgflags-dev -y
RUN apt-get install libatlas-base-dev -y
RUN apt-get install libeigen3-dev -y
RUN apt-get install libsuitesparse-dev -y
RUN apt install libceres-dev -y
RUN apt update -y
RUN apt install libgtsam-dev libgtsam-unstable-dev -y
RUN apt install libboost-all-dev -y
WORKDIR /root/catkin_ws/src/
RUN git clone https://github.com/Fadimee/FAST_LIO_LC.git --recursive
RUN apt install ros-melodic-eigen-conversions -y
RUN apt install ros-melodic-cv-bridge -y

#COPY . /root/catkin_ws/src/FAST_LIO_LC/

WORKDIR /root/catkin_ws/
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin_make'
RUN sed -i "6i source \"/root/catkin_ws/devel/setup.bash\"" /ros_entrypoint.sh

ENTRYPOINT ["/ros_entrypoint.sh"]
ENV DISPLAY=novnc:0.0
CMD ["bash"]