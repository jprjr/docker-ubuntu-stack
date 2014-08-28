FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:chris-lea/node.js && \
    apt-get update && \
    apt-get install -y nodejs python g++ make && \
    npm -g install forever && \
    apt-get remove -y manpages manpages-dev g++ gcc cpp make software-properties-common unattended-upgrades ucf g++-4.6 gcc-4.6 cpp-4.6

RUN useradd -m default && \
    mkdir -p /etc/s6/nodejs && \
    ln -s /bin/true /etc/s6/nodejs/finish

ADD nodejs.run /etc/s6/nodejs/run 
ADD nodejs.setup /etc/s6/nodejs/setup
