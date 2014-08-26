FROM jprjr/ubuntu-nginx:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN apt-get install -y curl && \
    cd /tmp && \
    curl -R -L -O https://download.elasticsearch.org/kibana/kibana/kibana-3.1.0.tar.gz && \
    tar xf kibana-3.1.0.tar.gz && \
    mv kibana-3.1.0/* /srv/www/ && \
    rm -rf /tmp/kibana-3.1.0*
