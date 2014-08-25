FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN export DEBIAN_FRONTEND=noninteractive && \
    echo " deb http://packages.elasticsearch.org/logstash/1.4/debian stable main" > /etc/apt/sources.list.d/mongodb.list && \
    sudo apt-key adv --fetch-keys "http://packages.elasticsearch.org/GPG-KEY-elasticsearch" && \
    apt-get update && \
    apt-get -y install logstash logstash-contrib rinetd

RUN mkdir -p /etc/s6/logstash && \
    mkdir -p /etc/s6/rinetd && \
    mkdir -p /etc/s6/udp_redirect && \
    ln -s /bin/true /etc/s6/logstash/finish && \
    ln -s /bin/true /etc/s6/rinetd/finish && \
    ln -s /bin/true /etc/s6/udp_redirect/finish && \
    rm /etc/s6/syslog/setup && \
    echo "*.*    @127.0.0.1" > /etc/syslog.conf

ADD logstash.run /etc/s6/logstash/run
ADD logstash.setup /etc/s6/logstash/setup

ADD rinetd.conf /etc/rinetd.conf
ADD rinetd.run /etc/s6/rinetd/run

ADD udp_redirect.run /etc/s6/udp_redirect/run

ADD udp_redirect /usr/sbin/udp_redirect
COPY conf /etc/logstash/conf.d
COPY conf /opt/logstash.default
COPY conf.opts /opt/logstash-confs

