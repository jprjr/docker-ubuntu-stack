FROM jprjr/ubuntu-nginx:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN cd /tmp && \
    curl -R -L -O https://download.elasticsearch.org/kibana/kibana/kibana-3.1.0.tar.gz && \
    tar xvf kibana-3.1.0.tar.gz && \
    mv -v kibana-3.1.0/* /srv/www && \
    sed -i 's/9200/80/' /srv/www/config.js && \
    rm -rf /tmp/kibana-3.1.0*

COPY root /
