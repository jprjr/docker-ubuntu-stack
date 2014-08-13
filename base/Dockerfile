FROM ubuntu:14.04
MAINTAINER John Regan <john@jrjrtech.com>

ADD logstash-forwarder_0.3.1_amd64.deb /tmp/logstash-forwarder_0.3.1_amd64.deb

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get install -y runit && \
    dpkg -i /tmp/logstash-forwarder_0.3.1_amd64.deb && \
    apt-get clean
