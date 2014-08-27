#jprjr/ubuntu-postfox:14.04

This is my attempt to run MongoDB in Docker.

## Needed Volumes

* `/srv/mongo/mongodb` - Used for storing the mongodb configuration.
* `/etc/mongod.conf` - Used for reading in your own configuration

## Ports

* 27017 - the standard mongodb port

## Environment Variables

### The complete list:

* `SYSLOG_HOST`

### Details on each variable

* `SYSLOG_HOST` - Forward all syslog messages to a host
