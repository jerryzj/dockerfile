FROM nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04
LABEL maintainer = "Jerry ZJ"

ENV DEBIAN_FRONTEND noninteractive

# Set apt mirror, using NCHC's server
RUN sed -i 's/archive.ubuntu.com/free.nchc.org.tw/g' \
    /etc/apt/sources.list
# Update
RUN apt-get update
RUN apt-get upgrade -y
# Install missing system-tools
RUN apt-get install software-properties-common locales -y
# Set locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
# Install build tools
RUN apt-get install -y \
    cmake \
    build-essential \
    device-tree-compiler
# Install LLVM toolchain
RUN apt-get install -y \
    clang \
    llvm  \
    lldb
# Install python3
RUN apt-get install -y \
    python3 python3-dev \
    python3-pip
# Install common tools
RUN apt-get install -y \
    vim git lftp tmux wget curl p7zip \
    clang-format clang-tidy \
    gdb valgrind gperf \
    fish
# Cleanup
RUN apt-get autoremove -y\
    && apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
CMD ["/usr/bin/fish"]
