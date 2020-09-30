FROM ubuntu:20.04

LABEL maintainer "Jeffrey Phillips Freeman <the@jeffreyfreeman.me>"

#ENV AMDGPUDRIVERFILE="amdgpu-pro-17.40-492261.tar.xz"
#ENV AMDGPUDIRNAME="amdgpu-pro-17.40-492261"

ENV AMDGPUDIRNAME="amdgpu-pro-20.20-1098277-ubuntu-20.04"
ENV AMDGPUDRIVERFILE="${AMDGPUDIRNAME}.tar.xz"

WORKDIR /tmp

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
    apt-get -y --no-install-recommends install \
      ca-certificates \
      wget \
      xz-utils && \
    wget --referer https://www.amd.com/en/support/kb/release-notes/rn-amdgpu-unified-linux-20-30 https://drivers.amd.com/drivers/linux/${AMDGPUDRIVERFILE} &&\
    tar -xvf $AMDGPUDRIVERFILE && rm $AMDGPUDRIVERFILE && \
    ./$AMDGPUDIRNAME/amdgpu-pro-install -y --compute && \
    rm -rf $AMDGPUDIRNAME && \
    apt-get install -y opencl-amdgpu-pro && \
    apt-get -y autoremove && \
    apt-get clean autoclean && \
    rm -rf /var/lib/{apt,cache,log}

ARG aparapiver
RUN mvn org.apache.maven.plugins:maven-dependency-plugin:2.1:get -DrepoUrl="https://repo1.maven.org/maven2/" -Dartifact=com.aparapi:aparapi:${aparapiver}

#RUN dpkg --add-architecture i386 && \
#    apt-get update && apt-get -y dist-upgrade  && \
#    apt-get -y --no-install-recommends install ca-certificates curl xz-utils

# only used to speedup build
#COPY $AMDGPUDRIVERFILE /tmp/    
#    curl -O https://download.b2go.cloud/AMD-GPU-PRO/$AMDGPUDRIVERFILE && \
#    apt-get -y remove ca-certificates wget xz-utils && \