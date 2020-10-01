FROM ubuntu:20.04

LABEL maintainer "Jeffrey Phillips Freeman <the@jeffreyfreeman.me>"

WORKDIR /tmp

RUN apt-get update && \
    apt-get -y dist-upgrade && \
    rm -rf /var/lib/{cache,log}

RUN apt-get update && \
    apt-get -y --no-install-recommends install \
      maven \
      git \
      openjdk-14-jre \
      openjdk-14-jdk && \
    rm -rf /var/lib/{cache,log}

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      ocl-icd-libopencl1 \
      ocl-icd-opencl-dev \
      pocl-opencl-icd \
      clinfo && \
    apt-get -y autoremove && \
    apt-get clean autoclean && \
    rm -rf /var/lib/{apt,cache,log}

ARG aparapiver
RUN mvn org.apache.maven.plugins:maven-dependency-plugin:2.1:get -DrepoUrl="https://repo1.maven.org/maven2/" -Dartifact="com.aparapi:aparapi:${aparapiver}"

RUN rm -rf /tmp/*

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
ENV JAVA_HOME /usr/lib/jvm/java-14-openjdk-amd64/
