################################
# Based on LFS v8
################################
FROM ubuntu
MAINTAINER Hervé Beraud <hervberaud.pro@gmail.com>

# install requirements
RUN apt-get update && \
    apt-get install -y gcc binutils make linux-source wget && \
    apt-get install -y bison m4 patch g++ gawk texinfo file xz-utils


WORKDIR /bin
RUN rm sh && \
    ln -s bash sh

# prepare environment
COPY version-check.sh .
COPY library-check.sh .

# check environment
RUN bash version-check.sh
RUN bash library-check.sh

ENV LFS=/mnt/lfs

RUN mkdir -pv $LFS/usr && \
    mkdir -pv $LFS/sources && \
    chmod -v a+wt $LFS/sources

WORKDIR $LFS/sources

COPY wget-list .
COPY md5sums .
# get packages list directly from the official repository
#RUN wget http://www.fr.linuxfromscratch.org/view/lfs-stable/wget-list
#RUN wget http://www.fr.linuxfromscratch.org/view/lfs-stable/md5sums
RUN wget --input-file=wget-list --continue
RUN pushd $LFS/sources && \
    md5sum -c md5sums && \
    popd

# Chapter 4.2
RUN mkdir -v $LFS/tools && \
    ln -sv $LFS/tools /

# Chapter 4.3
RUN groupadd lfs
    useradd -s /bin/bash -g lfs -m -k /dev/null lfs && \
    echo "lfs:lfs" | chpasswd

RUN chown -v lfs $LFS/tools && \
    chown -v lfs $LFS/sources && \

USER lfs

# Chapter 4.4
ENV LC_ALL=POSIX \
    LFS_TGT=x86_64-lfs-linux-gnu \
    PATH=/tools/bin:/bin:/usr/bin \
    MAKEFLAGS="-j $PROC"
