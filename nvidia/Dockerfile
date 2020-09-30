ARG aparapi-ver

FROM maven:3.6.3-openjdk-14

LABEL maintainer "Jeffrey Phillips Freeman <the@jeffreyfreeman.me>"

RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            ocl-icd-libopencl1 \
            ocl-icd-opencl-dev \
            clinfo && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /etc/OpenCL/vendors && \
    echo "libnvidia-opencl.so.1" > /etc/OpenCL/vendors/nvidia.icd

RUN mvn org.apache.maven.plugins:maven-dependency-plugin:2.1:get -DrepoUrl="https://repo1.maven.org/maven2/" -Dartifact=com.aparapi:aparapi:${aparapi-ver}

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
