#jprjr/ubuntu-nodejs

This is an image for running a basic NodeJS app - it uses `forever` to
auto-reload your NodeJS app as files update.

## How to use this image

This image runs in the `/home/default` folder as the `default` user. You should
use volumes to bring in your NodeJS app. You specify the script's path with
the `NODEJS_SCRIPT` environment variable, and arguments with the `NODEJS_SCRIPT_ARGS`
environment variable.

### Environment variables list

As usual, you can make a file at `/private/nodejs/vars` instead of declaring
environment variables:

* `NODEJS_SCRIPT` - the path to your NodeJS script
  * Note: this will be run from the `/home/default` directory - your
    path *must* be a relative path from that. If you use an absolute
    path, `forever` will try to monitor the entire filesystem (and fail).
* `NODEJS_SCRIPT_ARGS` - args to pass to your NodeJS script
* `NODEJS_NO_AUTOREFRESH` - set this to `1` if you don't want the script to autorefresh on files changing.

## Making derivations of this image

If you want to install a particular app, you'll want to make a new image sourcing
this one, install GCC, make etc, and go through your app's installation process.

One trick to keep your image from blowing up is to write out a build script,
and in your build script, install gcc, etc, install the app, then remove gcc
etc. This way, only one layer is committed.

If you use a bunch of `RUN` commands, like

```
RUN apt-get install -y gcc
RUN <whatever>
RUN apt-get remove -y gcc
```

Then each `RUN` command gets saved to disk - making that last `apt-get remove`
not really do anything.

Here's a handy cheat-sheet for installing a particular app - I'm going to use
[Ghost](https://github.com/tryghost/Ghost) as an example:

The important steps:

* Set a default script to run in `/etc/s6/nodejs/vars`
* Create patterns to ignore in `/home/default/.foreverignore`

First, the Dockerfile

```
FROM jprjr/ubuntu-nodejs:latest

ADD build.sh /tmp/build.sh
RUN /tmp/build.sh

EXPOSE 2368
```

Then, the script:

```
#!/usr/bin/env bash

apt-get update && apt-get install -y nodejs python g++ make git
su -c 'curl -L https://ghost.org/zip/ghost-latest.zip -o /home/default/ghost.zip' \
   -s /bin/bash default
su -c 'cd /home/default unzip -uo ghost.zip -d ghost' \
   -s /bin/bash default
su -c 'cd /home/default/ghost && npm install --production' \
   -s /bin/bash default
cp /home/default/ghost/config.example.js /home/default/ghost/config.js
sed -i 's/127.0.0.1/0.0.0.0/g' /home/default/ghost/config.js
chown default:default /home/default/ghost/config.js

apt-get remove -y g++ make git
apt-get autoremove -y

echo "NODEJS_SCRIPT=ghost/index.js" >> /etc/s6/nodejs/vars
echo "*.db" >> /home/default/.foreverignore
```

