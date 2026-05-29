FROM ubuntu:26.04

ARG USERNAME=lanzhiwang
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN set -eux; \
    apt update; \
    apt install -y locales software-properties-common ca-certificates tree iputils-ping vim wget curl iproute2 iptables lsof dnsutils sudo; \
    # Set locale
    locale-gen en_US en_US.UTF-8; \
    update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8; \
    # Enable required repositories
    add-apt-repository universe; \
    ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}'); \
    curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"; \
    dpkg -i /tmp/ros2-apt-source.deb; \
    rm -rf /tmp/ros2-apt-source.deb; \
    apt update; \
    apt install -y ros-dev-tools; \
    apt upgrade -y; \
    apt install -y ros-lyrical-ros-base ros-lyrical-desktop; \
    rm -rf /var/lib/apt/lists/*; \
    echo "alias ll='ls -alFi --color=auto'" >> /etc/bash.bashrc; \
    groupadd --gid $USER_GID $USERNAME; \
    useradd --uid $USER_UID --gid $USER_GID -m -s /bin/bash $USERNAME; \
    echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME; \
    chmod 0440 /etc/sudoers.d/$USERNAME

ENV LANG=en_US.UTF-8

USER $USERNAME
WORKDIR /home/$USERNAME
