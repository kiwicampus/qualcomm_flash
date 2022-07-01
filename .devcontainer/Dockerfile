# Pull ubuntu base image
FROM ubuntu:18.04

# Or your actual UID, GID on Linux if not the default 1000
ARG USERNAME=koda
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# For wifi configuration and settings
ENV DBUS_SYSTEM_BUS_ADDRESS=unix:path=/host/run/dbus/system_bus_socket

# -----------------------------------------------------------------------------
# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    # ------------------------------
    # Free memory
    && apt autoremove -y && apt clean -y \
    && rm -rf /var/lib/apt/lists/*

# Update
RUN apt-get update -y && apt-get install -y

# Install dependencies
RUN apt-get install -y unzip && apt-get install -y wget && apt-get install -y build-essential checkinstall && apt-get install -y sudo
RUN apt-get install -y openssh-server usbutils
RUN sudo apt-get install -y adb coreutils=8.28* coreutils fakechroot=2.19*  fakeroot=1.22* kmod libc6-arm64-cross qemu-user-static udev git

# Install tc-sdkmanager
COPY tc-sdkmanager-v2.1.0_amd64.deb /mnt/tc-sdkmanager-v2.1.0_amd64.deb
RUN sudo dpkg -i /mnt/tc-sdkmanager-v2.1.0_amd64.deb

# # Home directory setup
# RUN sudo useradd -rm koda -d /home/koda -s /bin/bash -u 1000
# RUN echo 'koda:123456' | chpasswd
# RUN sudo usermod -aG sudo koda
# RUN sudo usermod -aG plugdev koda
# WORKDIR /home/koda/

# SSH setup
RUN service ssh start
EXPOSE 22
CMD ["/usr/sbin/sshd","-D"]
