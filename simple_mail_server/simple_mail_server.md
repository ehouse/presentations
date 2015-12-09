% Simple Mail Server
% Ethan House (ehouse@csh.rit.edu)

# Email can be Easy

# Parts of the Email Stack

## SMTP
 - It accepts, relays, and forwards emails
 - This is what sends and receives emails between servers.
 - It does not present them for users, that's IMAP's job

## IMAP/POP3
 - Access emails remotely
 - IMAP keeps the files on server
 - POP3 downloads then deletes from server

# Setup of SMTP Server

## Why OpenSMTPD?
 - Made by the guys who made OpenBSD
 - Written to be secure and hard to mess up
 - Terse, easy to write config language

##
### Config

``` bash
	listen on egress port 25 tls pki ehouse.io auth-optional
	listen on egress port 587 tls-require pki ehouse.io auth mask-source

	table aliases file:/etc/mail/aliases

	# Receive scanned mails from amavisd-new
	listen on "127.0.0.1" port 10025 tag Scanned
	# Receive DKIM signed mails from amavisd
	listen on "127.0.0.1" port 10027 tag DKIM-Signed

	# Forward all mail received for local domains to amavis
	accept from any for domain "ehouse.io" relay via "smtp://127.0.0.1:10024"

	# Relay messages that are already signed
	accept tagged DKIM-Signed for any relay

	# Forward local deliver mails to DKIM signing service
	accept from local for any relay via "smtp://127.0.0.1:10026"

	# Deliver locally messages coming back in from scanner
	accept tagged Scanned from source "127.0.0.1" for domain "ehouse.io" alias <aliases> deliver to lmtp "/var/dovecot/lmtp"
```

##
### Config Breakdown - Accepting Connections

```bash
	listen on egress port 25 tls pki ehouse.io auth-optional
	listen on egress port 587 tls-require pki ehouse.io auth mask-source
	table aliases file:/etc/mail/aliases
```
	
 - Listen on SMTP and Submission port
 - SMTP is for intra server and Subbmion is to submit new emails
 - Create table alias for system aliases

##
### Config Breakdown - Listening and Scanning

``` bash
	# Receive scanned mails from amavisd-new
	listen on "127.0.0.1" port 10025 tag Scanned
	# Receive DKIM signed mails from amavisd
	listen on "127.0.0.1" port 10027 tag DKIM-Signed
```

 - Create two sockets for internal relaying
 - One for accepting Amavisd Scanned
 - One for Accepting DKIM Signed

##
### Config Breakdown - Internal Relaying

``` bash
	# Forward all mail received for local domains to amavis
	accept from any for domain "ehouse.io" relay via "smtp://127.0.0.1:10024"

	# Forward local deliver mails to DKIM signing service
	accept from local for any relay via "smtp://127.0.0.1:10026"

	# Relay messages that are already signed
	accept tagged DKIM-Signed for any relay
```

Accept all emails for domain "ehouse.io" and relay to Amavisd for scanning
Accept all email from amavisd and relay back for DKIM signing
Finally relay email outwards

##
### Config Breakdown - Delivery

``` bash
	# Deliver locally messages coming back in from scanner
	accept tagged Scanned from source "127.0.0.1" for domain "ehouse.io" alias <aliases> deliver to lmtp "/var/dovecot/lmtp"
```

 - Deliver all email that fufill the prior requirements of the config
 - Mail will be submitted to the lmtp socket for dovecot to deliver

<!-- ## Testing -->

<!-- # Setup of IMAP Server -->

<!-- ## About Dovecot -->

<!-- ## Config and Setup -->

<!-- ## Testing -->

<!-- # Mail Stack Add-ons -->

<!-- ## DKIM -->

<!-- ## SPF -->

<!-- ## Web Mail -->

<!-- # That's it -->

