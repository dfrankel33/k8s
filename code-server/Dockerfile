FROM mcr.microsoft.com/powershell:debian-10


RUN #!/bin/sh
 apt-get update \
 && apt-get install --no-install-recommends -y \
    curl \
    wget \
    dumb-init \
    htop \
    locales \
    man \
    nano \
    git \
    iputils-ping \
    iputils-tracepath \
    procps \
    ssh \
    sudo \
    vim \
    lsb-release \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*
  
ENV POWERSHELL_DISTRIBUTION_CHANNEL=PSDocker-TestDeps-Debian-10

# https://wiki.debian.org/Locale#Manually
RUN sed -i "s/# en_US.UTF-8/en_US.UTF-8/" /etc/locale.gen \
  && locale-gen
ENV LANG=en_US.UTF-8

RUN chsh -s /bin/bash
ENV SHELL=/bin/bash

RUN adduser --gecos '' --disabled-password coder && \
  echo "coder ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/nopasswd

RUN ARCH="$(dpkg --print-architecture)" && \
    curl -fsSL "https://github.com/boxboat/fixuid/releases/download/v0.4.1/fixuid-0.4.1-linux-$ARCH.tar.gz" | tar -C /usr/local/bin -xzf - && \
    chown root:root /usr/local/bin/fixuid && \
    chmod 4755 /usr/local/bin/fixuid && \
    mkdir -p /etc/fixuid && \
    printf "user: coder\ngroup: coder\n" > /etc/fixuid/config.yml

COPY release-packages/code-server*.deb /tmp/
RUN dpkg -i /tmp/code-server*$(dpkg --print-architecture).deb && rm /tmp/code-server*.deb

EXPOSE 8080
USER coder
WORKDIR /home/coder
ENTRYPOINT ["dumb-init", "fixuid", "-q", "/usr/bin/code-server", "--bind-addr", "0.0.0.0:8080", "."]

# Use PowerShell as the default shell
# Use array to avoid Docker prepending /bin/sh -c
CMD [ "pwsh" ]

ARG IMAGE_NAME=dfrankel33/code-server