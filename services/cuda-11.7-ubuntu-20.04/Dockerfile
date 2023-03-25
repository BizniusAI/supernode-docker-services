FROM nvidia/cuda:11.7.1-base-ubuntu20.04
LABEL maintainer = "An-Ting Hsu"

ENV DEBIAN_FRONTEND noninteractive
ARG ROOT_PASSWORD

# Set apt mirror, using NCHC's server
RUN sed -i 's/archive.ubuntu.com/free.nchc.org.tw/g' \
    /etc/apt/sources.list

# Update
RUN apt-get update
RUN apt-get upgrade -y

# Install missing system-tools
RUN apt-get install software-properties-common locales -y

# Set locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Install build tools
RUN apt-get install -y \
    cmake ninja-build \
    build-essential \
    device-tree-compiler

# Install python3
RUN apt-get install -y \
    python3 python3-dev \
    python3-pip

# Install common tools
RUN apt-get install -y \
    vim git tmux wget curl p7zip \
    clang-format clang-tidy \
    gdb valgrind fish

# Install ssh server
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

RUN echo "root:${ROOT_PASSWORD}" | chpasswd

RUN echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
RUN mkdir /root/.ssh

# Cleanup
RUN apt-get autoremove -y && apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
