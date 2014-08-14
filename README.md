# docker-ubuntu-stack

This is somewhat inspired by the [Phusion Docker image](https://phusion.github.io/baseimage-docker/). This git repo is going to be my collection of simple, usable Docker images.

Let's get into the "why" of this stack:

## Phusion has some good ideas

* If you're running multiple processes in your container, you need a proper init program.
* Some kind of syslog daemon is good to have.
* Having a cron daemon running is also a good idea, depending on what packages you're setting up.

## Parts of their ideas, I disagree with

* I don't think running SSH in a container is ever a good idea.
* Their init process is a custom-written init, done in Python.
  * This increases the size of the image, and prevents people from creating smaller utility images
* They include runit, so you essentially have two init daemons running
* I haven't gone through the code extensively, but I do know this:
  * `docker stop` sends a `SIGTERM` to PID 1
  * runit doesn't forward `SIGTERM` to processes it's monitoring.
  * So I'm not entirely sure how processes are getting TERM'd properly.

## How I'm planning to address that

Instead of a custom init + runit, I'm just using [s6](http://www.skarnet.org/software/s6/index.html). It's a set of process supervising programs.

There's a few benefits to using s6:

* If s6 gets a SIGTERM, I know it sends a SIGTERM to everything it's supervising. [It's right there in the documentation](http://www.skarnet.org/software/s6/s6-svscan.html)
* I can statically compile s6, so I can reuse it for other base images, including small utility images. There's no external library dependencies and minimal RAM usage.
* It's very easy to setup services - make a directory at `/etc/s6/` and place `run` and `finish` scripts.
  * (Or just symlink `finish` to `/bin/true`).
* I didn't write it, somebody who knows a lot about init did.

## Caveats

1. I'm pretty sure s6 has the ability to do dependency management, but I haven't figured that out yet.
  * In practice, I don't see this being a problem. I usually place dependent services into their own containers anyway.

## TODO

1. Add the syslog-ng service.
2. Look into statically compiling the logstash-forwarder daemon.
