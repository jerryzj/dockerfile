FROM ubuntu:latest
MAINTAINER jerryzj

ENV DEBIAN_FRONTEND noninteractive

RUN sed -i 's/archive.ubuntu.com/free.nchc.org.tw/g' \
    /etc/apt/sources.list

# Update system
RUN apt-get update
RUN apt-get upgrade -y

# Install essentials
RUN apt-get install -y git flex bison spim build-essential wget vim
# Install coolc compiler
RUN mkdir /usr/cs143
RUN chown root /usr/cs143
RUN cd /usr/cs143 && wget https://s3-us-west-1.amazonaws.com/prod-edx/Compilers/Misc/student-dist.tar.gz && tar -xvf student-dist.tar.gz && rm student-dist.tar.gz
RUN echo "export PATH=$PATH:/usr/cs143/bin" >> /root/.bashrc 
RUN ln -sf /usr/cs143/ ~/cool

# Cleanup
RUN apt-get autoremove && apt-get autoclean 
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENTRYPOINT ["/bin/bash"]
