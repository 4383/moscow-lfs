FROM ubuntu
MAINTAINER Hervé Beraud <hervberaud.pro@gmail.com>

# install requirements
RUN apt-get update && \
    apt-get install -y gcc binutils make linux-source && \
    apt-get install -y bison m4 patch g++ gawk texinfo file xz-utils


WORKDIR /bin
RUN rm sh && \
    ln -s bash sh

# prepare environment
COPY version-check.sh .

# check environment
RUN bash version-check.sh
