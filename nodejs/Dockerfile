FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:chris-lea/node.js && \
    apt-get update && \
    apt-get install -y nodejs python g++ make && \
    npm -g install forever && \
    npm -g install express && \
    npm -g install body-parser && \
    npm -g install request && \
    npm -g install faye && \
    npm -g install faye-websocket && \
    npm -g install redis && \
    npm -g install faye-redis && \
    npm -g install winston && \
    npm -g install winston-logstash && \ 
    npm -g install bunyan && \
    npm -g install bunyan-logstash && \ 
    npm -g install bunyan-logstash-tcp && \ 
    apt-get remove -y manpages manpages-dev g++ gcc cpp make python-software-properties unattended-upgrades ucf g++-4.6 gcc-4.6 cpp-4.6

RUN     useradd -m default

WORKDIR /home/default

USER    default
ENV     HOME /home/default
ENV     NODE_PATH /usr/lib/node_modules
RUN     touch /home/default/.foreverignore

ENTRYPOINT ["forever"]
