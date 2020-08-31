FROM ubuntu:18.04
MAINTAINER Dmitry Babokin <dmitry.y.babokin@intel.com>

ARG SHA=master

RUN apt-get -y update && apt-get install -y git build-essential curl file flex libc6-dev-i386 && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/gcc
ENV GCC_HOME=/usr/local/gcc

ARG HTTP_PROXY=
RUN git config --global --add http.proxy "$HTTP_PROXY"

RUN git clone https://gcc.gnu.org/git/gcc.git src
RUN mkdir build && mkdir bin
WORKDIR /usr/local/gcc/src
RUN git log --format="%H" -n 1 > ../rev.txt
RUN contrib/download_prerequisites
RUN CPU_CORES=`cat /proc/cpuinfo |grep processor |wc -l` contrib/gcc_build -d $GCC_HOME/src -o $GCC_HOME/build -c "--enable-multilib --prefix=$GCC_HOME/bin" -m -j$CPU_CORES configure build install
