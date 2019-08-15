FROM jerryzj/riscv:latest
LABEL maintainer = "jerryzj"

ENV DEBIAN_FRONTEND noninteractive


# Update system
RUN apt-get update
RUN apt-get upgrade -y

# Install System C library
RUN wget https://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.3.tar.gz && \
    tar -xvzf systemc-2.3.3.tar.gz && rm systemc-2.3.3.tar.gz
RUN cd systemc-2.3.3 && ./configure --prefix=/opt/systemc-2.3.3 && \
    export CXX=clang++ && export CXXFLAGS="std=c++14" && \
    make -j && make Install
RUN rm -rf /systemc-2.3.3

# Install SCML library

# Cleanup
RUN apt-get autoremove -y \
    && apt-get autoclean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log/*
