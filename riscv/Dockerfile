FROM jerryzj/dev_basic:latest
LABEL maintainer = "jerryzj"

ENV DEBIAN_FRONTEND noninteractive

# Update system
RUN apt-get update
RUN apt-get upgrade -y

# install SiFive riscv toolchain
# Note that the SiFive prebuilt toolchain is directly downloaded from 
# SiFive hosted server, and it may upgrade continuously.
RUN wget -q https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz && \
    tar -xvzf riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz && \
    rm riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz
RUN mv riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14 /opt/riscv-toolchain
# Set $PATH
RUN sed -e 's|PATH="\(.*\)"|PATH="/opt/riscv-toolchain/bin:\1"|g' -i /etc/environment
# Since docker build use sh as default, add ENV in order to build Spike later
ENV PATH=$PATH:/opt/riscv-toolchain/bin

# Build riscv-pk, ISS
# riscv-pk (Proxy Kernel)
# Update to v1.0.0 release
RUN wget -q https://github.com/riscv/riscv-pk/archive/v1.0.0.tar.gz && \
    tar -xvzf v1.0.0.tar.gz && rm v1.0.0.tar.gz
RUN cd riscv-pk-1.0.0 && mkdir build && cd build && \
    ../configure --prefix=/opt/riscv-toolchain --host=riscv64-unknown-elf && \
    make -j && make install
RUN rm -rf /riscv-pk-1.0.0
# Spike ISA Simulator
# Use Spike stable release 1.0.0 source files
RUN wget -q https://github.com/riscv/riscv-isa-sim/archive/v1.0.0.tar.gz && \
    tar -xvzf v1.0.0.tar.gz && rm v1.0.0.tar.gz
RUN cd riscv-isa-sim-1.0.0 && mkdir build && cd build && \
    ../configure --prefix=/opt/riscv-toolchain && \
    make && make install
RUN rm -rf /riscv-isa-sim-1.0.0

# Cleanup
RUN apt-get autoremove -y \
    && apt-get autoclean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log/*
