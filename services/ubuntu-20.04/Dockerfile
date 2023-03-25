FROM ubuntu:20.04
LABEL maintainer = "An-Ting Hsu"

ENV DEBIAN_FRONTEND noninteractive

# Set apt mirror, using NCHC's server
RUN sed -i 's/archive.ubuntu.com/free.nchc.org.tw/g' \
    /etc/apt/sources.list

# Update
RUN apt-get update
RUN apt-get upgrade -y

# Install missing system-tools
RUN apt-get install software-properties-common locales apt-utils -y

# Set locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Install basic tools
RUN apt-get install -y --no-install-recommends git curl bzip2 wget openssh-client p7zip \
    vim pandoc gpg-agent apt-transport-https ca-certificates
RUN add-apt-repository ppa:deadsnakes/ppa 

# Install build tools
RUN apt-get install -y --no-install-recommends build-essential bison bc flex file \
    help2man gperf texinfo gawk unzip zip autoconf automake autotools-dev \
    libncurses5-dev libssl-dev libxxhash-dev libtool

# Install python3
RUN apt-get install -y python3.9 python3.9-distutils
RUN ln -sf /usr/bin/python3.9 /usr/bin/python3
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && python3.9 get-pip.py && rm get-pip.py
RUN pip3 install --upgrade pip

# Install ssh server
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

RUN echo 'root:root' | chpasswd

RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config
RUN mkdir /root/.ssh

# Cleanup
RUN apt-get autoremove -y && apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
