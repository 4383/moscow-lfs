# mOScow

A minimal Linux From Scratch (LFS) distribution built-in docker container

## usage

### from docker hub
```shell
$ docker run 4383/moscow tar -cJf - -C /mnt/lfs . > lfs.tar.xz
$ # now you can create your own docker container that using the lfs system build
$ # previously by using the instruction FROM scratch in your dockerfile
$ # and by copying lfs.tar.xz inside
```
### from sources
```shell
$ git clone https://github.com/4383/moscow
$ cd sources
$ docker build -t moscow .
```

## build your own docker container based on your lfs
```yaml
FROM scratch

COPY lfs.tar.xz .
CMD /bin/bash
```
