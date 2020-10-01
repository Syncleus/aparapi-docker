![](http://aparapi.com/images/logo-text-adjacent.png)

[![License](http://img.shields.io/:license-apache-blue.svg?style=flat-square)](http://www.apache.org/licenses/LICENSE-2.0.html)
[![Gitter](https://badges.gitter.im/Syncleus/aparapi.svg)](https://gitter.im/Syncleus/aparapi?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

**Licensed under the Apache Software License v2**

This docker image serves as a means to easily test Aparapi based applications in a OpenCL and GPU enabled environment. Provided the host system has the appropriate hardware
Aparapi applications can utilize GPU acceleration from this container.

This docker image is based off the [Maven](https://hub.docker.com/_/maven) docker image, and adds either AMD, NVIDIA, or Intel opencl support on top depending on which tag you use.

## Support and Documentation

The official source repository for this project can be found on [QOTO GitLab](https://git.qoto.org/aparapi/aparapi-docker) an up-to-date mirror of the repository is also hosted on [Github](https://github.com/Syncleus/aparapi-docker).

For detailed documentation see [Aparapi.com](http://Aparapi.com).

For support please use [Gitter](https://gitter.im/Syncleus/aparapi) or the [official Aparapi mailing list and Discourse forum](https://discourse.qoto.org/c/PROJ/APA).

Please file bugs and feature requests on [QOTO GitLab](https://git.qoto.org/aparapi/aparapi-jni/issues) older issues are archived at [Github](https://github.com/Syncleus/aparapi/issues).

## Related Projects

This particular repository only represents the Aparapi Docker image. There are several other related repositories worth taking a look at.

* [Aparapi](https://git.qoto.org/aparapi/aparapi) - Core Aparapi java library.
* [Aparapi Examples](https://git.qoto.org/aparapi/aparapi-examples) - A collection of Java examples to showcase the Aparapi library and help developers get started.
* [Aparapi Native](https://git.qoto.org/aparapi/aparapi-native) - The native library component. Without this the Java library can't talk to the graphics card. This is not a java project but rather a C/C++ project.
* [Aparapi Vagrant](https://git.qoto.org/aparapi/aparapi-vagrant) - A vagrant environment for compiling aparapi native libraries for linux, both x86 an x64.
* [Aparapi Website](https://git.qoto.org/aparapi/aparapi.com) - Source for the Aparapi website as hosted at [http://aparapi.com](http://aparapi.com). The site also contains our detailed documentation.

## Obtaining the Source

The official source repository for Aparapi is located on the QOTO GitLab repository and can be cloned using the
following command.

```bash

git clone http://git.qoto.org/aparapi/aparapi-docker.git
```

## Building

This repo holds multiple versions of images depending on the opencl implementation being used. The following set of commands shows how to build each one.

```bash
docker build -t <image name> --build-arg "aparapiver=<aparapi Version>" amdgpu/
docker build -t <image name> --build-arg "aparapiver=<aparapi Version>" nvidia/

```

## Running

To run the amdgpu-pro image you must have compatible hardware and drivers installed. To do so execute the following command.

```bash
docker run --device /dev/dri -it aparapi/aparapi-amdgpu:latest bash
```

To run the NVIDIA based OpenCL implementation you can run the following command presuming you have the [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker) installed as well as the appropriate drivers.

```bash
docker run --runtime=nvidia -it aparapi/aparapi-nvidia:latest bash
```

### Using as a Gitlab Runner

If you wish to use this container as an image for use in a GitLab CI when setting up your own runner then the following config.toml will serve as an example. Ensure you select an AMI which has the 
[NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker) installed and the instance type is GPU-accelerated with NVIDIA hardware (most are). Both of which are the case in the below example. Don't forget
to also change the MachineOptions and other values to appropriate values.


```toml
[[runners]]
  name = "GPGPU Runner"
  url = "https://git.qoto.org/"
  token = "XXXXXXXXXXXXXXXXX"
  executor = "docker+machine"
  limit=1
  [runners.custom_build_dir]
  [runners.docker]
    tls_verify = false
    image = "aparapi/aparapi-nvidia:latest"
    privileged = true
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = true
    shm_size = 0
    runtime = "nvidia"
  [runners.cache]
    [runners.cache.s3]
      ServerAddress = "s3.wasabisys.com"
      AccessKey = "XXXXXXXXXXXXXXXXX"
      SecretKey = "XXXXXXXXXXXXXXXXX"
      BucketName = "git.qoto.org"
      BucketLocation = "us-east-1"
  [runners.machine]
    IdleCount = 0
    IdleTime = 1800
    MachineDriver = "amazonec2"
    MachineName = "gitlab-docker-machine-%s"
    MachineOptions = [
      "amazonec2-access-key=XXXXXXXXXXXXXXXX",
      "amazonec2-secret-key=XXXXXXXXXXXXXXXX",
      "amazonec2-vpc-id=vpc-0243bc5bc666df2e2",
      "amazonec2-subnet-id=subnet-0a43db1988ad60343",
      "amazonec2-use-private-address=true",
      "amazonec2-tags=runner-manager-name,gitlab-aws-autoscaler,gitlab,true,gitlab-runner-autoscale,true",
      "amazonec2-security-group=qoto-sq",
      "amazonec2-instance-type=p2.xlarge",
      "amazonec2-region=us-east-1",
      "amazonec2-zone=a",
      "amazonec2-ami=ami-06a25ee8966373068",
      "amazonec2-root-size=128",
    ]

```
