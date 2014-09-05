FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN DEBIAN_FRONTEND=noninteractive \
    apt-get update && apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:chris-lea/node.js && \
    apt-get update && \
    apt-get install -y nodejs python g++ make && \
    npm -g install forever && \
    apt-get remove -y g++ gcc cpp make software-properties-common && \
    apt-get autoremove -y && \
    useradd -m default

COPY root /
