#jprjr/ubuntu-base:14.04

This is just my base for some upcoming images. For details on the high-level/artistic decisions, take a look at my [stack's readme](https://github.com/jprjr/docker-ubuntu-stack/blob/master/README.md)

It's based on the stock `ubuntu` image, w/ `s6` and `logstash-forwarder`
installed.

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

## Caveats

Your process shouldn't fork/background. Keep it in the foreground.
