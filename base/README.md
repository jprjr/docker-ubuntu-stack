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

The logstash forwarder is disabled by default. It has a simple config file
for forwarding a single log file and giving it a `type` attribute.
If you'd like to use that, it's pretty easy to get the logstash forwarder
running. In your service's setup script, you can just:

* add `LOG_FILE_PATH` and `LOG_FILE_TYPE` to `/etc/s6/logstash-forwarder/vars`
* delete the `/etc/s6/logstash-forwarder/down` file
* run `s6-svc -u /etc/s6/logstash-forwarder`

I generally use `/private/<service>/vars` as a read-only mount - I don't
want my containers polluting up your `/private` space, hence why I'm using
`/etc/s6/logstash-forwarder`.

If you want something more advanced (like multiple log files), make your
own `logstash-forwarder.conf` file and make an environment variable named
`LOGSTASH_FORWARDER_SKIP_SETUP` and set it to `1` (so it doesn't try to edit your config file).

You'll also need to delete the `down` when you make your image, or you can delete
the `down` file and run `s6-svc -u /etc/s6/logstash-forwarder` at run-time.

Lastly, the logstash-forwarder requires you to setup an SSL CA file. There's
two ways you can do this:

* Have a file at `/private/ssl/ca.pem`
* Declare the `LOGSTASH_SSL_CA_URL` environment variable, and it will be downloaded.

If you don't do either of those things, then the setup script will just grab the
certificate provided by your logstash container and use that as its certificate
authority.

Here's an example from my nginx image, for forwarding `/var/log/nginx/access.log`

```bash
#!/usr/bin/env bash
declare LOG_FILE_PATH
declare LOG_FILE_TYPE

if [[ -f /private/nginx/vars ]]; then
  source /private/nginx/vars
fi

LOG_FILE_PATH=${LOG_FILE_PATH:-/var/log/nginx/*.log}
LOG_FILE_TYPE=${LOG_FILE_TYPE:-nginx}

echo "LOG_FILE_PATH=${LOG_FILE_PATH}" >> /etc/s6/logstash-forwarder/vars
echo "LOG_FILE_TYPE=${LOG_FILE_TYPE}" >> /etc/s6/logstash-forwarder/vars

rm /etc/s6/logstash-forwarder/down
s6-svc -u /etc/s6/logstash-forwarder
```

## Reference

### Links

Here's a list of links you can make, and what features that will turn on.

* `syslog` - syslog messages will be forwarded via udp on port 514.
* `rsyslog` - syslog messages will be forward via tcp port 2514 ([relp](http://www.rsyslog.com/doc/relp.html)).
* `logstash` - the logstash forwarder will forward logs to this container on port 5043

### Environment variables

Here's the complete list of environment variables you can set, and what they do.

You can also set these variables by mounting a file at `/private/syslog/vars`

* `RSYSLOG_SKIP_SETUP` - Setting this to one skips any and all setup of rsyslog - useful for using your own configuration via
   volumes

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

* `SYSLOG_STDOUT` - enable/disable syslog messages appearing on stdout by setting this to `1` or `0`.
  Messages will appear on stdout unless you link to a `syslog` or `rsyslog` host.
