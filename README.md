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
