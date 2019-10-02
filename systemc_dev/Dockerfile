FROM jerryzj/riscv:latest
LABEL maintainer = "jerryzj"

# Set environment variables
ENV SYSTEMC_HOME=/opt/systemc
ENV SCML_HOME=/opt/scml
ENV LD_LIBRARY_PATH=/opt/systemc/lib

# Update system
RUN apt-get update
RUN apt-get upgrade -y

# Install depedencies
RUN apt-get install -yq autoconf automake perl flex

# Install System C library
RUN wget -q https://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.3.tar.gz && \
    tar -xvzf systemc-2.3.3.tar.gz && rm systemc-2.3.3.tar.gz
RUN cd systemc-2.3.3 && mkdir build && cd build && \ 
    cmake .. -DCMAKE_CXX_STANDARD=14 && make install -j4

RUN rm -rf /systemc-2.3.3

# Install SCML library 2.5.0 (failed)
#RUN wget --no-check-certificate \
#    "https://drive.google.com/uc?export=download&id=1sRu0WRuhoXk_joiNzhcoWAnDW1HiZs_S" \
#    -O scml.tgz && tar -xvzf scml.tgz && rm scml.tgz
#RUN mkdir /opt/scml && mkdir /scml-2.5.0/build && cd /scml-2.5.0/build \
#    ../configure --prefix=/opt/scml --with-systemc=/opt/systemc-2.3.3 && \
#    make -j2 && make install 
#RUN rm -rf /scml-2.5.0

# Install SCML library 2.4.3
# Note that the configure of SCML is fixed to C++11 by default, but the C++ version
# should be bound to SystemC, for the hardcored version API
# We have a patch script to make SCML compatible with 
#   1. latest g++ 8.3
#   2. C++ 14 standard
#   3. System C 2.3.3 built by CMake (dynamic library .so instead of static library)
RUN git clone https://gitlab.larc-nthu.net/rgly/scml.git \
    && mkdir /scml/build \
    && mkdir /opt/scml
RUN cd /scml && sh /scml/scml_patch.sh
RUN cd /scml/build \
    && ../configure --prefix=${SCML_HOME} --with-systemc=${SYSTEMC_HOME} \
    && make install -j4
RUN rm -rf /scml

# Install riscv-llvm 32bit toolchain
# Here I use my own prebuilt version to save time
RUN wget -q https://github.com/jerryzj/riscv-llvm-toolchain/releases/download/v1.0/andes-rv32-toolchain-v1.0.tar.xz && \
    tar Jxvf andes-rv32-toolchain-v1.0.tar.xz && rm andes-rv32-toolchain-v1.0.tar.xz && \
    mv andes-rv32-toolchain /opt/andes-rv32-toolchain
# Set PATH
RUN sed -e 's|PATH="\(.*\)"|PATH="/opt/andes-rv32-toolchain/bin:\1"|g' -i /etc/environment
ENV PATH=$PATH:/opt/andes-rv32-toolchain/bin

# Install Arch C
#ENV ARCHC_HOME=/opt/archc-2.4.1
#
#RUN mkdir -p ${ARCHC_HOME} && \
#    git clone https://github.com/ArchC/ArchC.git ArchC
#RUN cd ArchC && ./autogen.sh && \
#    ./configure --prefix=${ARCHC_HOME} --with-systemc=${SYSTEMC_HOME} && \
#    make install -j2 && cd / && rm -rf ArchC
#RUN sed -e 's|PATH="\(.*\)"|PATH="/opt/archc-2.4.1/bin:\1"|g' -i /etc/environment
#ENV PATH=$PATH:/opt/archc-2.4.1/bin

# Install Arch C modified version
ENV ARCHC_HOME=/opt/archc-mod
RUN wget -q https://github.com/jerryzj/ArchC-mod/releases/download/v1.0/archc-mod-v1.0.tar.xz && \
    tar Jxvf archc-mod-v1.0.tar.xz && rm archc-mod-v1.0.tar.xz && \
    mv archc-mod /opt/archc-mod
# Set PATH
RUN sed -e 's|PATH="\(.*\)"|PATH="/opt/archc-mod/bin:\1"|g' -i /etc/environment
ENV PATH=$PATH:/opt/archc-mod/bin

# For compatibility (to old SystemC builds & EEWS)
RUN ln -sf /opt/systemc/lib/ /opt/systemc/lib-linux64
RUN echo 'SYSTEMC_HOME=/opt/systemc' >> /etc/environment && \
    echo 'SCML_HOME=/opt/scml'>> /etc/environment && \
    echo 'ARCHC_HOME=/opt/archc-mod'>> /etc/environment

# Cleanup
RUN apt-get autoremove -y \
    && apt-get autoclean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log/*
