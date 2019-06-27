FROM jerryzj/dev_basic:latest
MAINTAINER jerryzj

ENV DEBIAN_FRONTEND noninteractive

# Update system
RUN apt-get update
RUN apt-get upgrade -y

# install SiFive riscv toolchain
RUN wget https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.2.0-2019.05.3-x86_64-linux-ubuntu14.tar.gz
RUN tar -xvzf riscv64-unknown-elf-gcc-8.2.0-2019.05.3-x86_64-linux-ubuntu14.tar.gz
RUN rm riscv64-unknown-elf-gcc-8.2.0-2019.05.3-x86_64-linux-ubuntu14.tar.gz
RUN mv riscv64-unknown-elf-gcc-8.2.0-2019.05.3-x86_64-linux-ubuntu14 /opt/riscv-toolchain
RUN echo "export PATH=$PATH:/opt/riscv-toolchain/bin" >> /root/.bashrc  
RUN /bin/bash -c "source /root/.bashrc"
ENV PATH=$PATH:/opt/riscv-toolchain/bin

# build riscv-pk, ISS
# riscv-pk
RUN git clone https://github.com/riscv/riscv-pk.git
RUN cd riscv-pk && git checkout 97b683e && mkdir build && cd build && \
    ../configure --prefix=/opt/riscv-toolchain --host=riscv64-unknown-elf && \
    make -j && make install
RUN rm -rf /riscv-pk
# Spike ISS
RUN wget https://github.com/riscv/riscv-isa-sim/archive/v1.0.0.tar.gz
RUN tar -xvzf v1.0.0.tar.gz && cd riscv-isa-sim-1.0.0 && \
    mkdir build && cd build && ../configure --prefix=/opt/riscv-toolchain && \
    make && make install
RUN rm -rf v1.0.0.tar.gz riscv-isa-sim-1.0.0
# Cleanup
RUN apt-get autoremove -y \
    && apt-get autoclean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
