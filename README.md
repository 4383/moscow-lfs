# mOScow

A minimal Linux From Scratch (LFS) distribution built-in docker container

## usage

### from docker hub
```shell
$ docker run 4383/moscow tar -cJf - -C /mnt/lfs . > lfs.tar.xz
```
### from sources
```shell
$ git clone https://github.com/4383/moscow
$ cd sources
$ docker build -t moscow .
```
