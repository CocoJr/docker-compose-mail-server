# Docker Compose Mailbox

Fully featured mail server using docker-compose

Postfix, dovecot, vimbadmin and roundcube, all in one convenient package.

Includes opendkim for postfix

**Thanks to https://github.com/frankh/docker-compose-mailbox, it's just a fork**

## Info

The docker compose file runs 6 services

  1. postfix - handle's sending and receiving email
  2. dovecot - sorts incoming mail into IMAP mailboxes
  3. vimbadmin - web interface for creating/managing mailboxes
  4. roundcube - web interface for checking a mailbox
  5. opendkim - Adds domainkey validation so that sent mails aren't flagged as spam
  6. spamassassin - Automatically filters spam for inboxes - learns based on what users mark as spam/not spam

postfix needs port **25\*** and **587\*** to be open

dovecot needs port **993\***

vimbadmin and roundcube need port **80**

**\*Or port defined in `.env` file.**

## Usage Instructions

The instructions assume you are trying to setup an email server for `example.com`

First copy the `.env.dist` to `.env` and edit it.`

To run the server, use `docker-compose up -d` (-d is for daemon mode, so it runs in the background)

Once you've run it, set up the DNS entries
listed in the next section, and then use vimbadmin to set up your mail boxes

## Summary of DNS entries

You will need to setup the following DNS entries to get everything working. (Assuming all options above are left as the
default)

  1. MX Record - `example.com` must have an MX Record pointing to `mail.example.com`
  2. A or CNAME Record - `mail.example.com` must point to the server running the docker service
  3. A or CNAME Record - `vimbadmin.example.com` must point to the server running the docker service
  4. A or CNAME Record - `webmail.example.com` must point to the server running the docker service
  5. TXT Record - `mail._domainkey.example.com` must be set to the contents given in `./data/certs/dkim.txt`
     (Created after first run)
  6. TXT Record - `example.com` should be set to `v=spf1 mx a:mail.example.com ~all`

## Certificates and keys

Upon running for the first time some certificates and keys will be created in `./data/certs`. If you want to use keys
and certificates you have created, place them in here. The dkim private key must be in `./data/certs/dkim.private`, the
mail server key and certificate must be in `./data/certs/mail.private` and `./data/certs/mail.cert`.

## Backups

Backups will be created every 24 hours for the roundcube/vimbadmin databases and for the dovecot vmail directory

They will be stored in ./data/backup and deleted after 1 week - it's recommended to automatically back these up on a
separate server.

Please, refer to https://github.com/frankh/docker-compose-mailbox for copyright
