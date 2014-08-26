# jprjr/ubuntu-dnsmasq:14.04

An image with dnsmasq installed.

You should mount /etc/dnsmasq.conf as a volume (and optionally /etc/dnsmasq.d).

## Using this image:

First, if you need a dnsmasq.conf file --

```bash
$ docker run -v /path/to/new/dnsmasq.conf:/etc/dnsmasq.conf --entrypoint /opt/copyconf.sh jprjr/ubuntu-dnsmasq:14.04
```

Then, after editing dnsmasq.conf:

```bash
$ docker run -p 53:53 -v /path/to/new/dnsmasq.conf:/etc/dnsmasq.conf jprjr/ubuntu-dnsmasq:14.04
```

Now you can do some neat stuff - like run another container, and use the `--dns` flag to have
local DNS.
