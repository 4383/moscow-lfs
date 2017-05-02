# mOScow
[![Build Status](https://travis-ci.org/4383/moscow.svg?branch=master)](https://travis-ci.org/4383/moscow)

A minimal Linux From Scratch (LFS) distribution built-in docker container

[Project page](https://github.com/4383/moscow)

## usage
### from docker hub
```shell
$ docker run 4383/moscow tar -cJf - -C /mnt/lfs . > lfs.tar.xz
$ # now you can create your own docker container that using the lfs system build
$ # previously by using the instruction FROM scratch in your dockerfile
$ # and by copying lfs.tar.xz inside
$ mkdir my-lfs-based-container
$ cp lfs.tar.xz my-lfs-based-container/
$ cd my-lfs-based-container
$ cat > Dockerfile << "EOF"
FROM scratch
ADD lfs.tar.xz .
EOF
$ docker build -t mylfs .
$ docker exec -i -t mylfs /bin/bash
```

### from sources
```shell
$ git clone https://github.com/4383/moscow
$ cd sources
$ docker build -t moscow .
$ docker run moscow tar -cJf - -C /mnt/lfs . > lfs.tar.xz
$ # now you can create your own docker container that using the lfs system build
$ # previously by using the instruction FROM scratch in your dockerfile
$ # and by copying lfs.tar.xz inside
$ mkdir my-lfs-based-container
$ cp lfs.tar.xz my-lfs-based-container/
$ cd my-lfs-based-container
$ cat > Dockerfile << "EOF"
FROM scratch
ADD lfs.tar.xz .
EOF
$ docker build -t mylfs .
$ docker exec -i -t mylfs /bin/bash
```

## build your own docker container based on your lfs
Now you have generate your own lfs based distribution use it inside your
docker containers
```yaml
FROM scratch
COPY lfs.tar.xz .
CMD /bin/bash
```
You can modify lfs building by adding your own utilities, like a specific python
interpreter for your project and build your flavor.

## advantages
- lightweight system
- flavored container
- specific usages

## maintainer
Hervé Beraud (4383)
