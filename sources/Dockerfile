################################
# Based on LFS v8
################################
FROM ubuntu
ENV LFS_TEST=1
ENV PROC=1
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
RUN groupadd lfs &&\
    useradd -s /bin/bash -g lfs -m -k /dev/null lfs && \
    echo "lfs:lfs" | chpasswd && \
    chown -v lfs $LFS/tools && \
    chown -v lfs $LFS/sources
USER lfs

# Chapter 4.4
COPY .bash_profile /home/lfs/
COPY .bashrc /home/lfs/
ENV LC_ALL=POSIX \
    LFS_TGT=x86_64-lfs-linux-gnu \
    PATH=/tools/bin:/bin:/usr/bin \
    MAKEFLAGS="-j $PROC"

# Chapter 5
# Chapter 5.4
RUN tar -xf binutils-2.27.tar.bz2 -C /tmp/ && \
    pushd /tmp/binutils-2.27 && \
    mkdir -v build && \
    cd build && \
    ../configure \
        --prefix=/tools \
        --with-sysroot=$LFS \
        --with-lib-path=/tools/lib \
        --target=$LFS_TGT \
        --disable-nls \
        --disable-werror && \
    make && \
    mkdir -pv /tools/lib && \
    ln -sv lib /tools/lib64 && \
    make install && \
    popd && \
    rm -rf /tmp/binutils-*
RUN echo $LFS
RUN echo $LFS_TGT
RUN ls -la $LFS
RUN pwd
RUN ls
# Chapter 5.5
RUN tar -xf gcc-6.3.0.tar.bz2 -C /tmp/ && \
    pushd /tmp/gcc-6.3.0 && \
    tar -xf $LFS/sources/mpfr-3.1.5.tar.xz && \
    mv -v mpfr-3.1.5 mpfr && \
    tar -xf $LFS/sources/gmp-6.1.2.tar.xz && \
    mv -v gmp-6.1.2 gmp && \
    tar -xf $LFS/sources/mpc-1.0.3.tar.gz && \
    mv -v mpc-1.0.3 mpc && \
    for file in gcc/config/{linux,i386/linux{,64}}.h; do \
      cp -uv $file{,.orig}; \
      sed -e 's@/lib\(64\)\?\(32\)\?/ld@/tools&@g' \
          -e 's@/usr@/tools@g' $file.orig > $file; \
      echo -e '\n#undef STANDARD_STARTFILE_PREFIX_1\n#undef STANDARD_STARTFILE_PREFIX_2\n#define STANDARD_STARTFILE_PREFIX_1 "/tools/lib/"\n#define STANDARD_STARTFILE_PREFIX_2 ""' >> $file; \
      touch $file.orig; \
    done && \
    mkdir -v build && \
    cd build && \
    ../configure \
        --target=$LFS_TGT \
        --prefix=/tools \
        --with-glibc-version=2.11 \
        --with-sysroot=$LFS \
        --with-newlib \
        --without-headers \
        --with-local-prefix=/tools \
        --with-native-system-header-dir=/tools/include \
        --disable-nls \
        --disable-shared \
        --disable-multilib \
        --disable-decimal-float \
        --disable-threads \
        --disable-libatomic \
        --disable-libgomp \
        --disable-libquadmath \
        --disable-libssp \
        --disable-libvtv \
        --disable-libstdcxx \
        --enable-languages=c,c++ && \
    make && \
    make install && \
    popd && \
    rm -rf /tmp/gcc-*
