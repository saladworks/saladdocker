FROM ubuntu:22.04
LABEL org.opencontainers.image.description="Mercury image"

RUN apt-get update && apt-get install -y --no-install-recommends \
        openssh-server \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir /run/sshd \
    && useradd -m -s /bin/bash ubuntu \
    && echo 'ubuntu:ubuntu' | chpasswd

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
