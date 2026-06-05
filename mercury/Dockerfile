FROM ubuntu:22.04
LABEL org.opencontainers.image.description="Mercury image"

RUN apt-get update && apt-get install -y --no-install-recommends \
        openssh-server \
        sudo \
        iputils-ping \
        curl \
        python3 \
        python3-pip \
        iproute2 \
        git \
        gcc \
        g++ \
        cmake \
        gdb \
        build-essential \
        make \
        lsb-release \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir /run/sshd \
    && useradd -m -s /bin/bash ubuntu \
    && echo 'ubuntu:ubuntu' | chpasswd \
    && usermod -aG sudo ubuntu

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
