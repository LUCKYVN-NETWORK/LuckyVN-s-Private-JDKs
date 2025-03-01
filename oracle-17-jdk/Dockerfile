# ----------------------------------
# Pterodactyl Core Dockerfile
# Environment: Java
# Minimum Panel Version: 1.7.0
# ----------------------------------
FROM ubuntu:22.04

ARG TARGETPLATFORM
ARG GRAAL_VERSION=21.3.12
ARG JAVA_VERSION=17

LABEL       author="SunshroomChan" maintainer="viethoangluunguyen1974@gmail.com"

ENV DEBIAN_FRONTEND=noninteractive

# Default to UTF-8 file.encoding
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

RUN apt-get update -y \
  && apt-get install -y curl ca-certificates openssl git tar sqlite3 fontconfig tzdata locales iproute2 \
  && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
  && locale-gen en_US.UTF-8 \
  && case ${TARGETPLATFORM} in \
         "linux/amd64")  ARCH=linux-amd64  ;; \
         "linux/arm64")  ARCH=linux-aarch64  ;; \
    esac \
  && curl --retry 3 -Lfso /tmp/java17.tar.gz https://github.com/LUCKYVN-NETWORK/LuckyVN-s-Private-JDKs/raw/refs/heads/main/jdk-17.0.13_linux-x64_bin.tar.gz \
  && mkdir -p /opt/java/java17oracle \
  && cd /opt/java/java17 \
  && tar -xf /tmp/java17.tar.gz --strip-components=1 \
  && export PATH="/opt/java/java17/bin:$PATH" \
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf /tmp/java17.tar.gz

ENV JAVA_HOME=/opt/java/java17oracle \
    PATH="/opt/java/java17oracle/bin:$PATH"

# Step 2 - add pterodactyl stuff
RUN useradd -d /home/container -m container

USER        container
ENV         USER=container HOME=/home/container

WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh

CMD         ["/bin/bash", "/entrypoint.sh"]
