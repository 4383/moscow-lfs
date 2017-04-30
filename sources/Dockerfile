FROM ubuntu
MAINTAINER Hervé Beraud <hervberaud.pro@gmail.com>

# install requirements
RUN apt-get update

# prepare environment
COPY version-check.sh .

# check environment
RUN bash version-check.sh
