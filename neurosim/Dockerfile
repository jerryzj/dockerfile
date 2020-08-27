FROM jerryzj/dev_basic_gpu:latest
LABEL maintainer = "Jerry ZJ"

ENV DEBIAN_FRONTEND noninteractive

# Update
RUN apt-get update
RUN apt-get upgrade -y
RUN pip3 install torch==1.0.1 torchvision==0.2.2 --no-cache --user
# Cleanup
RUN apt-get autoremove -y\
    && apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
CMD ["/usr/bin/fish"]
