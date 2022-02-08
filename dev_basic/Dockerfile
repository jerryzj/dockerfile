FROM ubuntu:20.04
LABEL maintainer = "Jerry ZJ"

ENV DEBIAN_FRONTEND noninteractive

# Set apt mirror, using NCHC's server
RUN sed -i 's/archive.ubuntu.com/free.nchc.org.tw/g' \
    /etc/apt/sources.list
# Update
RUN apt-get update
RUN apt-get upgrade -y
# Install missing system-tools
RUN apt-get install software-properties-common locales apt-utils -y
# Set locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
# Install basic tools
RUN apt-get install -y --no-install-recommends git curl bzip2 wget openssh-client p7zip \
    clang-format fish vim pandoc gpg-agent apt-transport-https ca-certificates
RUN add-apt-repository ppa:ubuntu-toolchain-r/test
RUN add-apt-repository ppa:deadsnakes/ppa 
# Install build tools
RUN apt-get install -y --no-install-recommends build-essential bison bc flex file \
    help2man gperf texinfo gawk unzip zip autoconf automake autotools-dev \
    libncurses5-dev libssl-dev libxxhash-dev libtool
# Install LLVM toolchain
RUN wget https://apt.llvm.org/llvm.sh && chmod +x llvm.sh && ./llvm.sh 13 && rm llvm.sh
RUN update-alternatives --install /usr/bin/clang clang /usr/bin/clang-13 130 --slave /usr/bin/clang++ clang++ /usr/bin/clang++-13
RUN update-alternatives --install /usr/bin/lld lld /usr/bin/lld-13 130
# Install CMake
RUN wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null \
    | gpg --dearmor - | tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null
RUN apt-add-repository "deb https://apt.kitware.com/ubuntu/ $(lsb_release -cs) main"
RUN apt-get install -y --no-install-recommends cmake
# Install new ninja manually
# (https://www.claudiokuenzler.com/blog/756/install-newer-ninja-build-tools-ubuntu-14.04-trusty)
RUN wget https://github.com/ninja-build/ninja/releases/download/v1.10.2/ninja-linux.zip
RUN unzip ninja-linux.zip -d /usr/local/bin/ && rm ninja-linux.zip
RUN update-alternatives --install /usr/bin/ninja ninja /usr/local/bin/ninja 1 --force
# Install python3
RUN apt-get install -y python3.9 python3.9-distutils
RUN ln -sf /usr/bin/python3.9 /usr/bin/python3
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && python3.9 get-pip.py && rm get-pip.py
RUN pip3 install --upgrade pip
# Cleanup
RUN apt-get autoremove -y\
    && apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
CMD ["/usr/bin/fish"]
