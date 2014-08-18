#jprjr/ubuntu-spamassassin:14.04

This is my attempt to setup Spamassassin in Docker. Inspired by [mailinabox](https://github.com/mail-in-a-box/mailinabox).

## How to use this darn thing?

Well right now, I'm still working on getting other images setup, so I haven't
fully tested this yet. But here's what should work:

* spampd is listening to port `10025`
* spampd needs to connect to a dovecot server via lmtp on port `10026`

## Needed Volumes

* `/var/lib/spamassassin` - Used for storing spamassassin updates etc

## Optional Volumes

* `/etc/rsyslog.d` - If you want to customize your rsyslog

## Ports

* 10025 - An smtpproxy port for Postfix

## Environment Variables

### The complete list:

* `DOVECOT_HOST` (or link a container named `dovecot`)
* `SYSLOG_HOST` (or link a container named `syslog`)

### Details on each variable

* `IMAP_HOST` - What imap server to deliver mail to
* `SYSLOG_HOST` - use this to specify your syslog host (optional)
