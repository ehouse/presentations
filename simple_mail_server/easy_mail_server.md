% Quick and Easy Mail Server
% Ethan House (ehouse@csh.rit.edu)

## Email is Easy

. . .

I Promise

# Parts of the Email Stack
## SMTP
- PostFix
- OpenSMTPD

Its accepts and sends emails

## IMAP/POP3
Allows users to read and markup emails

The interface between the user and the email server

- Dovecot
- hMail
- Cyrus

# Setup of SMTP Server

## Why not Postfix?
The common goto server is Postfix

. . .

It's terrible

. . .

Incredable verbose and VERY easy to mess up

## Why OpenSMTPD?
Written to be easy to use

Written to be secure

Terse, easy to write config language

## Example Config Length
<-- Config Length Example here -->

## Config
<-- Example Config here ->

## Config Breakdown
<-- Go over Each line of config here -->

## Testing
<-- Connect with Mutt --> 

# Setup of IMAP Server
## About Dovecot
Hardest Part of Mail stack

I'll break it down for easy use

## Config and Setup
## Testing

# Mail Stack Add-ons
## DKIM
DomainKeys Identified Mail

Signs emails to check against 
## SPF
## Web Mail
