#jprjr/ubuntu-dovecot:14.04

This is my attempt to setup Dovecot in Docker. Inspired by [mailinabox](https://github.com/mail-in-a-box/mailinabox).

**This is still very experimental - do not use this in production!**

## How to use this darn thing?

Well right now, I'm still working on getting other images setup (like postfix, spamassassin, etc), so I haven't fully tested this yet. But here's what
I do know:

* Mail is stored in /srv/mail/mailboxes
* Sieve filters are stored at /srv/mail/sieve
* An SSL cert and key is required, they should be at `/private/ssl/ssl_certificate.pem` and `/private/ssl/ssl_private_key.pem`

At startup, I read everything from /srv/ssl and copy it to /srv/mail/ssl/
and `chown` it to the Dovecot user.

Also at startup, I check if `/etc/dovecot` is empty. If so, a default
config is copied into `/etc/dovecot`

If `/etc/dovecot` was empty and you either:

1. Have a container linked in as `mysql`
2. Have an environment variable named `MYSQL_HOST`

Then I also try to setup the database (more details below, under "Environment Variables"

## Needed Volumes

* `/srv/mail` - Used for storing mail
* `/private/ssl` - Used for reading in the SSL cert and key you want to use

## Optional Volumes

* `/etc/dovecot` - Used for storing the dovecot configuration. This will
be rebuilt using environment variables at startup, but this is a handy way
to do more customization.
* `/private/vars.sh` - You can place any of the listed environment variables
in this file and they'll be sourced at startup.

## Ports

* 143 - The standard IMAP port
* 993 - IMAP over SSL port
* 12345 - a SASL port for authentication against Dovecot
* 10026 - an LMTP port, mostly for SpamAssassin to use

## Environment Variables

### The complete list:

* `MYSQL_HOST`
* `MYSQL_USER`
* `MYSQL_PASS`
* `DOVECOT_SQL_USER`
* `DOVECOT_SQL_PASS`
* `ADMIN_EMAIL`
* `SKIP_DB_SETUP`
* `SKIP_DOVECOT_SETUP`
* `SYSLOG_HOST`

### Details on each variable

* `MYSQL_HOST` - use this to specify your MySQL server.
* `MYSQL_USER` - use this to specify your root MySQL user (optional)
* `MYSQL_PASS` - use this to specify your root MySQL password (optional)

If `MYSQL_HOST` is set (or you have a linked container named `mysql`), then
that MySQL server will be used for querying users.

If `MYSQL_USER` and `MYSQL_PASS` are set, then I'll create the needed
database for dovecot. If these are blank, I skip that step.

* `DOVECOT_SQL_USER`
* `DOVECOT_SQL_PASS`

These drive what database name to use, as well as a username and password
for that database. These are required variables.

* `ADMIN_EMAIL` - This is for creating a "postmaster" email address.

* `SKIP_DB_SETUP` - if this is set to anything, then the entire DB setup
process will be skipped. This is useful if you already have a db of users
you want to use, and don't want me trying to create tables.

* `SKIP_DOVECOT_SETUP` - Disables creating the /etc/dovecot folder.

* `SYSLOG_HOST` - Forward syslogs to this host.
