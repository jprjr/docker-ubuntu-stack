FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN export DEBIAN_FRONTEND=noninteractive && \
    echo " deb http://packages.elasticsearch.org/logstash/1.4/debian stable main" > /etc/apt/sources.list.d/mongodb.list && \
    sudo apt-key adv --fetch-keys "http://packages.elasticsearch.org/GPG-KEY-elasticsearch" && \
    apt-get update && \
    apt-get -y install logstash logstash-contrib

RUN mkdir -p /etc/s6/logstash && \
    ln -s /bin/true /etc/s6/logstash/finish

ADD logstash.run /etc/s6/logstash/run
ADD logstash.setup /etc/s6/logstash/setup
COPY conf /etc/logstash/conf.d
COPY conf.opts /opt/logstash-confs

