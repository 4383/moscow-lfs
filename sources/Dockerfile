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

RUN mkdir -pv $LFS/{usr, sources} && \
    chmod -v a+wt $LFS/sources

WORKDIR $LFS/sources

COPY wget-list .
RUN wget --input-file=wget-list --continue 
