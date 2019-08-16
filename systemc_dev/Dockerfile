FROM jerryzj/riscv:latest
LABEL maintainer = "jerryzj"

ENV DEBIAN_FRONTEND noninteractive
ENV SYSTEMC_HOME=/opt/systemc
ENV SCML_HOME=/opt/scml

# Update system
RUN apt-get update
RUN apt-get upgrade -y
# Install dependencies
RUN apt-get install -y libncurses5-dev autoconf automake perl libtool \
    libmpc-dev libmpfr-dev libgmp-dev gawk bison flex \
    texinfo patchutils zlib1g-dev pkg-config -y

# Install System C library
RUN wget https://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.3.tar.gz && \
    tar -xvzf systemc-2.3.3.tar.gz && rm systemc-2.3.3.tar.gz
RUN cd systemc-2.3.3 && mkdir build && cd build && \ 
    cmake .. -DCMAKE_CXX_STANDARD=14 && make install
RUN rm -rf /systemc-2.3.3

# Install SCML library 2.5.0 (failed)
#RUN wget --no-check-certificate \
#    "https://drive.google.com/uc?export=download&id=1sRu0WRuhoXk_joiNzhcoWAnDW1HiZs_S" \
#    -O scml.tgz && tar -xvzf scml.tgz && rm scml.tgz
#RUN mkdir /opt/scml && mkdir /scml-2.5.0/build && cd /scml-2.5.0/build \
#    ../configure --prefix=/opt/scml --with-systemc=/opt/systemc-2.3.3 && \
#    make -j && make install 
#RUN rm -rf /scml-2.5.0

# Install SCML library 2.4.3
# Note that the configure of SCML is fixed to C++11 by default, but the C++ version
# should be bound to SystemC, for the hardcored version API
# We have a patch script to make SCML compatible with 
#   1. latest g++ 8.3
#   2. C++ 14 standard
#   3. System C 2.3.3 built by CMake (dynamic library .so instead of static library)
ENV CXX=g++-8
RUN git clone https://gitlab.larc-nthu.net/rgly/scml.git \
    && mkdir /scml/build \
    && mkdir /opt/scml
RUN cd /scml && sh /scml/scml_patch.sh
RUN cd /scml/build \
    && ../configure --prefix=${SCML_HOME} --with-systemc=${SYSTEMC_HOME} \
    && make install
RUN rm -rf /scml

# Install Arch C
ENV PREFIX="${PREFIX:-/usr/local/lib}"
ENV ARCHC_VERSION=${ARCHC_VERSION:-2.4.1}
ENV ARCHC_HOME=${PREFIX}/archc-${ARCHC_VERSION}

RUN echo "ArchC version == ${ARCHC_VERSION}" && \
    echo "Installation path is ${ARCHC_HOME}"
RUN mkdir -p ${ARCHC_HOME} && \
    git clone https://github.com/ArchC/ArchC.git ArchC
RUN cd ArchC && ./autogen.sh && \
    ./configure --prefix=${ARCHC_HOME} --with-systemc=${SYSTEMC_HOME} && \
    make install && cd / && rm -rf ArchC

# Cleanup
RUN apt-get autoremove -y \
    && apt-get autoclean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log/*