FROM ubuntu:20.04

LABEL maintainer "Jeffrey Phillips Freeman <the@jeffreyfreeman.me>"

ENV AMDGPUDIRNAME="amdgpu-pro-21.10-1247438-ubuntu-20.04"
ENV AMDGPUDRIVERFILE="${AMDGPUDIRNAME}.tar.xz"

WORKDIR /tmp

RUN apt-get update && \
#    apt-get -y dist-upgrade && \
    rm -rf /var/lib/{cache,log}

RUN apt-get update && \
    apt-get -y --no-install-recommends install \
      maven \
      git \
      openjdk-14-jre \
      openjdk-14-jdk && \
    rm -rf /var/lib/{cache,log}

RUN apt-get update && \
    apt-get -y --no-install-recommends install \
      ca-certificates \
      wget \
      initramfs-tools \
      xz-utils && \
    wget --referer https://www.amd.com/en/support/kb/release-notes/rn-amdgpu-unified-linux-20-30 https://drivers.amd.com/drivers/linux/${AMDGPUDRIVERFILE} &&\
    tar -xvf $AMDGPUDRIVERFILE && rm $AMDGPUDRIVERFILE && \
    ./$AMDGPUDIRNAME/amdgpu-pro-install -y --compute && \
    rm -rf $AMDGPUDIRNAME && \
    apt-get install -y \
      clinfo \
      opencl-amdgpu-pro && \
    apt-get -y autoremove && \
    apt-get clean autoclean && \
    rm -rf /var/lib/{apt,cache,log}

ARG aparapiver
RUN mvn org.apache.maven.plugins:maven-dependency-plugin:2.1:get -DrepoUrl="https://repo1.maven.org/maven2/" -Dartifact=com.aparapi:aparapi:${aparapiver}

RUN rm -rf /tmp/*

ENV JAVA_HOME /usr/lib/jvm/java-14-openjdk-amd64/
