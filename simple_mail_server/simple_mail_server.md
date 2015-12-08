% Simple Mail Server
% Ethan House (ehouse@csh.rit.edu)

## Email can be Easy

. . .

I Promise

# Parts of the Email Stack
## SMTP
It accepts, relays, and forwards emails

This is what sends and receives emails between servers.

It does not present them for users, that's IMAP's job

## IMAP/POP3
Access emails remotely

IMAP keeps the files on server

POP3 downloads then deletes from server

# Setup of SMTP Server

## Why OpenSMTPD?
Made by the guys who made OpenBSD

Written to be secure and hard to mess up

Terse, easy to write config language

## Config
<!-- Example Config here -->

## Config Breakdown
<!-- Go over Each line of config here -->

## Testing
<!-- Connect with Mutt -->

# Setup of IMAP Server

## About Dovecot
## Config and Setup
## Testing

# Mail Stack Add-ons
## DKIM
## SPF
## Web Mail
