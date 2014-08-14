FROM ubuntu:14.04
MAINTAINER John Regan <john@jrjrtech.com>

ADD logstash-forwarder_0.3.1_amd64.deb /tmp/logstash-forwarder_0.3.1_amd64.deb
ADD s6-1.1.3.2-musl-static.tar /

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get install -y openssl ca-certificates && \
    dpkg -i /tmp/logstash-forwarder_0.3.1_amd64.deb && \
    apt-get clean && \
    mkdir -p /etc/s6/svc
