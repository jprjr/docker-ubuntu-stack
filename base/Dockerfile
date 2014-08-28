FROM ubuntu:14.04
MAINTAINER John Regan <john@jrjrtech.com>

ADD logstash-forwarder_0.3.1_amd64.deb /tmp/logstash-forwarder_0.3.1_amd64.deb
ADD s6-1.1.3.2-musl-static.tar /

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get install -y openssl ca-certificates \
     rsyslog rsyslog-relp cron curl rsync logrotate && \
    dpkg -i /tmp/logstash-forwarder_0.3.1_amd64.deb && \
    apt-get clean && \
    mkdir -p /etc/s6/.s6-svscan && \
    ln -s /bin/true /etc/s6/.s6-svscan/finish && \
    mkdir /etc/s6/cron && \
    mkdir /etc/s6/syslog && \
    mkdir /etc/s6/logstash-forwarder && \
    ln -s /bin/true /etc/s6/cron/finish && \
    ln -s /bin/true /etc/s6/syslog/finish && \
    ln -s /bin/true /etc/s6/logstash-forwarder/finish && \
    touch /etc/s6/logstash-forwarder/down && \
    rm -f /etc/rsyslog.conf && \
    rm -rf /etc/rsyslog.d && \
    mkdir /etc/logrotate.jprjr.d && \
    mkdir -p /etc/logstash/ssl

ADD cron.run /etc/s6/cron/run
ADD syslog.setup /etc/s6/syslog/setup
ADD syslog.run /etc/s6/syslog/run
ADD rsyslog.conf /etc/rsyslog.conf
ADD logrotate.conf /etc/logrotate.conf
ADD logstash-forwarder.conf /etc/logstash-forwarder.conf
ADD logstash-forwarder.run /etc/s6/logstash-forwarder/run
ADD logstash-forwarder.setup /etc/s6/logstash-forwarder/setup

RUN chmod 0644 /etc/logrotate.conf

ENTRYPOINT ["/usr/bin/s6-svscan","/etc/s6"]
CMD []
