# Dovecot

## Introduction
I was using dovecot for
several years on my raspi
without ever having to deal
with it. But I had to update
the system because raspbian
stretch was end of life, a fact that I had ignored way
too long, simply because I 
was not aware of it.

So I ignored this pretty long
until they finslly removed
the repos from the official
server.

Now I was more or less
forced to upgrade to buster.
Despite some hickups during
upgrade it went remarkably
well.

The only thing that did
no longer work was: *dovecot.*
 It started without any 
 notice, all daemons were
 running.

 But retrieving mails using
 IMAP failed. 

 It turned out that the 
 default security level
 for SSL/TLS has been raised
 from 1 to 2. This makes
 the system safer, as it
 no longer accepts weak
 ciphers for SSL/TLS.
 Good thing, but unfortunately
 dovecot obviously uses a
 too short key in the dh.pem
 parameter file. The error
 message in the mail 
 logfile was pretty clear
 about that, complaining
 "key too short".

 Several posts in the
 Internet suggested to
 reduce the security level
 to 1 again. *That is a
 bad idea* 
 (even if it works) 
 as it weakens
 the swcurity of *ALL* 
 SSL/TLS connections, not
 only for dovecot.

 So, what to do? Easy, just
 use a longer key with 4096
 bytes. Since the key is 
 contained in dh.pem you
 just have to create a 
 version of it with a
 longer key. The only 
 problem with that:
 dh.pem consists of (base64-
 encoded) binary data.
 Quite some posts later I
 learned that one can
 easily generate such a
 so-called parameter file
 using a openssl-command like
> openssl dhparams 4096\>dh4096.pem

 Well, did I really write *"easily"*?

 While the command itself is simple
 it takes *VERY, VERY, VERY*
 long time to complete,
 because it needs to
 compute very large prime-numbers, not exactly a key 
 funtion of raspis.
 As I didn't know before, I
 did not exactly time the
 command, but I know that it
 tortured my raspi for 13
 more hours after I realized it will take longer than
 I thought.

 So this repo contains
## dh4096.pem
 which I created with the
 above command, because I
 do not want to see raspis
 tortured all over the world
 just because of dovecot!