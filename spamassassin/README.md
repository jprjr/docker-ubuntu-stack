#jprjr/ubuntu-spamassassin:14.04

This is my attempt to setup Spamassassin in Docker. Inspired by [mailinabox](https://github.com/mail-in-a-box/mailinabox).

## How to use this darn thing?

Well right now, I'm still working on getting other images setup, so I haven't
fully tested this yet. But here's what should work:

* spampd is listening to port `10025`
* spampd needs to connect to an imap server via lmtp on port `10026`
* You need an environment variable named `IMAP_HOST`
* If there's an environment variable named `SYSLOG_SERVER` then syslog
messages will be forwarded to that address, port 514.

## Needed Volumes

* `/var/lib/spamassassin` - Used for storing spamassassin updates etc

## Optional Volumes

* `/etc/rsyslog.d` - If you want to customize your rsyslog

## Ports

* 10025 - An smtpproxy port for Postfix

## Environment Variables

### The complete list:

* `IMAP_HOST`
* `SYSLOG_HOST`

### Details on each variable

* `IMAP_HOST` - What imap server to delivery mail to
* `SYSLOG_HOST` - use this to specify your syslog host (optional)
