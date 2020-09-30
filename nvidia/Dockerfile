FROM ubuntu:20.04

LABEL maintainer "Jeffrey Phillips Freeman <the@jeffreyfreeman.me>"

RUN apt-get update && \
    apt-get -y dist-upgrade && \
    rm -rf /var/lib/{cache,log}

RUN apt-get update && \
    apt-get -y --no-install-recommends install \
      maven \
      openjdk-14-jre \
      openjdk-14-jdk && \
    rm -rf /var/lib/{cache,log}

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      ocl-icd-libopencl1 \
      ocl-icd-opencl-dev \
      clinfo && \
    apt-get -y autoremove && \
    apt-get clean autoclean && \
    rm -rf /var/lib/{apt,cache,log}

RUN mkdir -p /etc/OpenCL/vendors && \
    echo "libnvidia-opencl.so.1" > /etc/OpenCL/vendors/nvidia.icd

ARG aparapiver
RUN mvn org.apache.maven.plugins:maven-dependency-plugin:2.1:get -DrepoUrl="https://repo1.maven.org/maven2/" -Dartifact="com.aparapi:aparapi:${aparapiver}"

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
