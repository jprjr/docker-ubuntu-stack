FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN export DEBIAN_FRONTEND=noninteractive && \
    echo " deb http://packages.elasticsearch.org/logstash/1.4/debian stable main" > /etc/apt/sources.list.d/mongodb.list && \
    sudo apt-key adv --fetch-keys "http://packages.elasticsearch.org/GPG-KEY-elasticsearch" && \
    apt-get update && \
    apt-get -y install logstash logstash-contrib rinetd && \
    rm /etc/s6/syslog/setup

COPY root /
