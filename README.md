![](http://aparapi.com/images/logo-text-adjacent.png)

[![pipeline status](http://git.qoto.org/aparapi/aparapi-docker/badges/2.0.0/pipeline.svg)](http://git.qoto.org/aparapi/aparapi-docker/-/commits/2.0.0)
[![SemVer](https://img.shields.io/badge/SemVer-v2.0.0-green)](https://semver.org/spec/v2.0.0.html)
[![License](http://img.shields.io/:license-apache-blue.svg?style=flat-square)](http://www.apache.org/licenses/LICENSE-2.0.html)
[![Gitter](https://badges.gitter.im/Syncleus/aparapi.svg)](https://gitter.im/Syncleus/aparapi?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

**Licensed under the Apache Software License v2**

This docker image serves as a means to easily test Aparapi based applications in a OpenCL and GPU enabled environment. Provided the host system has the appropriate hardware
Aparapi applications can utilize GPU acceleration from this container.

This docker image is based off the [Maven](https://hub.docker.com/_/ubuntu) docker image, and adds either AMD, NVIDIA, or pocl opencl dev support on top depending on which image you use. In addition it also adds OpenJDK-14, both JRE
and JDK, git, maven, and predownloads the Aparapi Java dependency into the .M2 cache. It should be everything you need to run GPU accelerated Aparapi applications.

## Support and Documentation

The official source repository for this project can be found on [QOTO GitLab](https://git.qoto.org/aparapi/aparapi-docker) an up-to-date mirror of the repository is also hosted on [Github](https://github.com/Syncleus/aparapi-docker).

For detailed documentation see [Aparapi.com](http://Aparapi.com).

For support please use [Gitter](https://gitter.im/Syncleus/aparapi) or the [official Aparapi mailing list and Discourse forum](https://discourse.qoto.org/c/PROJ/APA).

Please file bugs and feature requests on [QOTO GitLab](https://git.qoto.org/aparapi/aparapi-docker/issues).

### Tags

* *`latest`* - Will always be update-to-date reflecting the latest version of Aparapi released and may occasionaly reflect an updated base system such as newer version of OpenCL or ubuntu. This will track the master branch
of this repository.

* *`git`* - This will be any development versions of the images that exist, often, though not always, tracking the latest snapshot of aparapi. This will track the develop branch of this repostory

* *`<Aparapi version>`* - Tags of this format, for example "2.0.0" will always be the latest version of the image for the given Aparapi version, in the example given the latest revision for Aparapi c2.0.0. These tags track
the branch with the same name in this repository.

* *`<Aparapi version>-<revision>`* - Tags of this format, for example "2.0.0-1" will be fixed releases and the image is garunteed never to change. It will reflect the Docker image for the specific Aparapi version and revision number.
Revsions will be released sequentially, as needed, as updated to the Docker image become availible. These tags will track the git tag with the same name in this repository.

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
docker build -t <image name> --build-arg "aparapiver=<aparapi Version>" pocl/

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

To run pocl, which only supports CPU processing at the moment, run the following.

```bash
docker run -it aparapi/aparapi-pocl:latest bash
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
