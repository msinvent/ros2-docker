FROM ros:foxy

LABEL maintainer ="Manish Sharma<msinvent@gmail.com>"

# Have takes the following dependencies from masato-ka/ros2-docker 
RUN apt update && \
    apt install -y --no-install-recommends python3-pip python3-apt && \
    apt install -y --no-install-recommends ros-foxy-gazebo-ros-pkgs ros-foxy-rviz2 && \
    apt install -y --no-install-recommends xvfb x11vnc x11-apps && \
    pip3 install -U setuptools colcon-common-extensions colcon-ros-bundle && \
    rm -rf /var/lib/apt/lists/*
RUN echo '/opt/ros/foxy/lib/python3.8/site-packages' > /usr/local/lib/python3.8/dist-packages/.pth
RUN apt update && apt install -y curl
RUN curl http://repo.ros2.org/repos.key | sudo apt-key add -
RUN curl http://packages.osrfoundation.org/gazebo.key | sudo apt-key add -

# Install some extra packages to avoid rosdep updates
# But, its not recommended to install extra packages than required in production
RUN apt update && \
    apt install -y --no-install-recommends ros-foxy-desktop ros-foxy-ros-testing ros-foxy-ros2test
# To install further missing packages from a directory colcon workspace
# rosdep install --from-paths <src-path> --ignore-src -r -y

RUN apt update && \
    apt install -y --no-install-recommends wget gitk tmux







#########################################################################################################
#########################################################################################################
# Setup new a non super user for improved safety
# user and password defaults to ubuntu, 
# but it is highly recommended to setup your own password for any serious usage
# Change user and encrypted password in the following command while building
# To generate encrypted password run
# perl -e 'print crypt("mypassword123", "salt-pepper-chilly"),"\n"'
# The output of above command in this case for me is 
# sayVb7E97UXnw0G7zp/L.SKg
# So building the docker with the following command will create a user named ubuntu with password mypassword123
# docker build . -t msinvent/ros2-foxy --build-arg user=ubuntu --build-arg pass=sayVb7E97UXnw0G7zp/L.SKg 

ARG user
ARG pass

ENV USER=${user:-ubuntu}
RUN echo "$USER"
RUN echo "${pass:-sahffl6VP523k}" 
RUN useradd --create-home --home-dir /home/${user:-ubuntu} --shell /bin/bash \
    -p ${pass:-sahffl6VP523k} --user-group --groups adm,sudo ${user:-ubuntu}

USER ${USER}
WORKDIR /home/${USER}

RUN rosdep update

# Command to run the container
# docker container run --rm -it -v $HOME:$HOME --name myowncontainer msinvent/ros2-foxy