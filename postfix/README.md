#jprjr/ubuntu-postfox:14.04

This is my attempt to setup Postfix in Docker. Inspired by [mailinabox](https://github.com/mail-in-a-box/mailinabox).

**This is still very experimental. Please do not run this in production!**

## How to use this darn thing?

Well right now, I'm still working on getting other images setup, so I haven't fully tested this yet. But here's what
I do know:

* The postfix config is in /etc/postfix

At startup, if the `/etc/postfix` directory is empty, a setup script is run. Said
setup script assumes:

* Your users are stored in a MySQL DB, using the setup from jprjr/ubuntu-dovecot:14.04
* You want SASL authentication against a dovecot instance
* You'll be running mail through spampd

## Needed Volumes

* `/etc/postfix` - Used for storing the postfix configuration.
* `/private/ssl` - Used for reading in the SSL cert and key you want to use

## Optional volumes

* `/private/vars.sh` - Used to setup environment variables from a file, instead
of on the command line.

## Ports

* 25 - The standard smtp port
* 587 - the standard submission port

## Environment Variables

### The complete list:

* `MYSQL_HOST` (not needed if there's a linked container named `mysql`)
* `DOVECOT_HOST` (not needed if there's a linked container named `dovecot`)
* `SPAMASSASSIN_HOST` (not needed if there's a linked container named `spamassassin`)
* `SYSLOG_HOST` (optional. Can also link a container named `syslog`)
* `DOVECOT_SQL_USER`
* `DOVECOT_SQL_PASS`
* `ADMIN_EMAIL`
* `SMTP_HOSTNAME`
* `SKIP_POSTFIX_SETUP`

### Details on each variable

* `MYSQL_HOST` - use this to specify your MySQL server.

* `DOVECOT_SQL_USER`
* `DOVECOT_SQL_PASS`

These drive what database name to use, as well as a username and password
for that database. These are required variables.

* `SPAMASSASSIN_HOST` - use this to specify your SpamAssassin host.

* `ADMIN_EMAIL` - This is for creating a "postmaster" email address.
  * This can also set the SMTP hostname - it will be smtp.<domain>

* `SMTP_HOSTNAME` - Set an explicit hostname for Postfix to use.

* `SYSLOG_HOST` - Forward all syslog messages to a host

* `SKIP_POSTFIX_SETUP` - Everytime the container starts, the setup is recreated from scrach using environment variables. Setting this to anything (0, 1, whatever) will keep that step from happening.
