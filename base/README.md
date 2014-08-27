#jprjr/ubuntu-base:14.04

This is just my base for some upcoming images. For details on the high-level/artistic decisions, take a look at my [stack's readme](https://github.com/jprjr/docker-ubuntu-stack/blob/master/README.md)

It's based on the stock `ubuntu` image, w/ `s6`, `logstash-forwarder`,
`cron`, and `rsyslog` installed.

## How to use this image

This image runs `s6-svscan /etc/s6` as its `ENTRYPOINT`. This means it scans
`/etc/s6` for service directories. There's more details from the author of s6
available [here](http://www.skarnet.org/software/s6/servicedir.html), but in
summary:

1. Make a folder at `/etc/s6/<servicename>`
  * Example: `/etc/s6/nginx`
2. Make a script at `/etc/s6/<servicename>/run`
  * Example: `/etc/s6/nginx/run`
  * Your `run` script should setup and launch your process - attach to your process with `exec` and make sure it won't try to background/fork.
  * If your process doesn't need any arguments, you could just symlink
    the binary instead of writing a script.
4. Make a script at `/etc/s6/<servicename>/finish`
  * Example: `/etc/s6/nginx/finish`
  * The finish script should clean up anything it needs to.
  * If you're lazy, just do `ln -sf /bin/true /etc/s6/<servicename>/finish`

That's it! S6 will launch your service, monitor it, and relaunch it if it dies.

## Pre-installed software and options

So a concept I really like is letting links turn features on and off.  
By linking to containers with specific names, you can enable/disable things.

### Syslog forwarding options

By default, rsyslog will write everything to stdout.

If you link to a host named `syslog`, the local syslog daemon will forward all
logs to that host on port 514 via udp. You can also define a host with the
`SYSLOG_HOST` environment variable, if you want to link as another name (or not
link at all!).

You can change the port by setting the `SYSLOG_CLIENT_PORT` to a port you'd prefer,
or set it to `0` to disable forwarding.

If you'd prefer to use TCP, you can set the `SYSLOG_CLIENT_PROTOCOL` environment variable
to `tcp`.

If you link to a host named `rsyslog`, then the syslog daemon will use rsyslog's
`relp` connection type on port 2514. You can also explicitly define a host with
the `RSYSLOG_HOST` environment variable. The port can be changed with `RSYSLOG_CLIENT_PORT`

If you use syslog forwarding, by default rsyslog will stop printing to stdout. You
can force this to stay on by setting `SYSLOG_STDOUT` to `1`.

### Syslog server options

You can also enable rsyslog to recieve forwarded syslogs with environment variables:

`SYSLOG_SERVER` will enable UDP and TCP input on port 514. You can set the
`SYSLOG_SERVER_UDP` and `SYSLOG_SERVER_TCP` environment variables to different
ports, or set them to `0` to disable that particular protocol.

`RSYSLOG_SERVER` will enable RELP input on port 2514. You can set the
`RSYSLOG_SERVER_PORT` environment variable to use a different port.

### Logrotate

Logrotate is setup to run once every day. Instead of reading files from
the standard `/etc/logrotate.d` folder, it reads files from the
`/etc/logrotate.jprjr.d` folder. You will need to write custom logrotate
rules for your packages/programs.

### Logstash forwarder

The logstash forwarder is disabled by default - if you have a service,
and you'd like to use the logstash forwarder, you'll need to edit the
`/etc/logstash-forwarder.conf` file and enable the service.

Here's an example from my nginx image:

```bash
# First, check for 'LOGSTASH_HOST' or a link named 'logstash'
declare LOGSTASH_HOST

LOGSTASH_HOST=${LOGSTASH_HOST:-}
LOGSTASH_NAME=${LOGSTASH_NAME:-}

if [[ -n ${LOGSTASH_NAME} ]]; then
  LOGSTASH_HOST=logstash
fi

if [[ -n ${LOGSTASH_HOST} ]]; then

  LOGSTASH_PORT=${LOGSTASH_PORT:-5043}
  LOG_FILE_PATH="/var/log/nginx/*.log"
  LOG_FILE_TYPE="nginx"

  sed -i "s/##LOGSTASH_HOST##/$LOGSTASH_HOST/g" /etc/logstash-forwarder.conf
  sed -i "s/##LOGSTASH_PORT##/$LOGSTASH_PORT/g" /etc/logstash-forwarder.conf
  sed -i "s|##LOG_FILE_PATH##|$LOG_FILE_PATH|g" /etc/logstash-forwarder.conf
  sed -i "s/##LOG_FILE_TYPE##/$LOG_FILE_TYPE/g" /etc/logstash-forwarder.conf

  # remote the 'down' file for s6
  rm /etc/s6/logstash-forwarder/down
  # tell s6 to bring up the service
  s6-svc -u /etc/s6/logstash-forwarder
fi
```

## Reference

### Links

Here's a list of links you can make, and what features that will turn on.

* `syslog` - syslog messages will be forwarded via udp on port 514.
* `rsyslog` - syslog messages will be forward via tcp port 2514 ([relp](http://www.rsyslog.com/doc/relp.html)).

### Environment variables

Here's the complete list of environment variables you can set, and what they do:

* `SYSLOG_HOST` - syslog messages will be forwarded via udp on port 514 to this host.
* `RSYSLOG_HOST` - syslog messages will be forward via tcp port 2514 ([relp](http://www.rsyslog.com/doc/relp.html)) to this host.
  * In the case that you define a variable named `SYSLOG_HOST` / `RSYSLOG_HOST` *and* a link, the link will be used and
    the `_HOST` variable ignored.

* `SYSLOG_CLIENT_PORT` - specify an alternate port to connect to
* `SYSLOG_CLIENT_PROTOCOL` - this can be set to `tcp` or `udp`, defaults to `udp`

* `RSYSLOG_CLIENT_PORT` - specify an alternative port for relp

* `SYSLOG_SERVER` - enable the syslog server, defaults to listening on port 514 via tcp and udp.
* `SYSLOG_SERVER_UDP` - change the port for incoming udp syslog traffic, or set to `0` to disable.
* `SYSLOG_SERVER_TCP` - change the port for incoming tcp syslog traffic, or set to `0` to disable.

* `RSYSLOG_SERVER` - enable the relp server, defaults to listening on port 2514 via tcp
* `RSYSLOG_SERVER_PORT` - change the port for incoming relp traffic, or set to `0` to disable.

* `SYSLOG_STDOUT` - force syslog messages to appear on stdout
