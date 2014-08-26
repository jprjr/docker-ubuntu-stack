FROM ubuntu:14.04
MAINTAINER John Regan <john@jrjrtech.com>

ADD logstash-forwarder_0.3.1_amd64.deb /tmp/logstash-forwarder_0.3.1_amd64.deb
ADD s6-1.1.3.2-musl-static.tar /

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get install -y openssl ca-certificates \
      rsyslog cron && \
    dpkg -i /tmp/logstash-forwarder_0.3.1_amd64.deb && \
    apt-get clean && \
    mkdir -p /etc/s6/.s6-svscan && \
    ln -s /bin/true /etc/s6/.s6-svscan/finish && \
    mkdir /etc/s6/cron && \
    mkdir /etc/s6/syslog && \
    ln -s /bin/true /etc/s6/cron/finish && \
    ln -s /bin/true /etc/s6/syslog/finish && \
    rm -rf /etc/rsyslog.d && \
    rm -f /etc/rsyslog.conf

ADD cron.run /etc/s6/cron/run
ADD syslog.setup /etc/s6/syslog/setup
ADD syslog.run /etc/s6/syslog/run

COPY conf/rsyslog /etc/

ENTRYPOINT ["/usr/bin/s6-svscan","/etc/s6"]
CMD []
