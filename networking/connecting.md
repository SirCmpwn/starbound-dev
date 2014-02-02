---
title: Connection Process
layout: base
---

# Connection Process

A client connects to a server through an exchange of packets, initiated by the server. The full process is documented in the following sections.

## Overview
There are five packets involved in the connection process. They are:

1. [Protocol Version](/networking#protocol-version) 
2. [Client Connect](/networking#client-connect)
3. [Handshake Challenge](/networking#handshake-challenge)
4. [Handshake Response](/networking#handshake-response)
5. [Connection Response](/networking#connection-response)

All five of these packets must be exchanged for a clean connection, even one that the server ends up refusing. **No** other packets may be sent during this period, or the connection will inevitably fail. 

The packets are covered below in the context of implementation. See also: [data types](/networking/data-types.html)

## Packets
Please note that all pseudo-code below will be operating on the extracted payload. The ID and Payload Size of each base packet is not factored in.

{% include connect-packet-header.html name="Protocol Version" id="0" direction="server-to-client" %}

This packet is sent by the server following a successful TCP handshake. The only data this contains is a protocol version, which is a 32-bit integer. The payload in hex from Angry Koala is `00000274`. This changes with each version.

After the client verifies the version, it sends a Client Connect packet.

{% include connect-packet-header.html name="Client Connect" id="6" direction="client-to-server"%}
The client connect packet is by far the most complex packet in terms of the handshake process. It is far larger than any of the others and contains the player info and a full shipworld file. The structure of the file is plotted in the Data Structures page, and the format of the files themselves is covered in the [File Formats](/file_formats/) section.

{% include connect-packet-header.html name="Handshake Challenge" id="03" direction="server-to-client" %}
The handshake challenge provides a claim message (currently unused), salt, and round count for the player to hash a password against. The handshake response is the password hash using these parameters. Due to the nature of this packet and the handshake response it is not possible to emulate a client without implementing the two packets dynamically, at a bare minimum.

The round count, in the current version, is set to 5000. Presumably this will be modifiable in the future, and indeed if you manually send the packets the client will respond with the correct hash. The salt is a randomly generated 32 character Base 64 string.

Example data for the handshake challenge packet:

* Salt: 5uAciRZkmwKUkek3krU+s2LTvPHE6v2P
* Claim Message: '' 
* Round Count: 5000

{% include connect-packet-header.html name="Handshake Response" id="8" direction="client-to-server" %}
The handshake response hash is generated using the SHA-256 algorithm, successively working on its own output. The format of the string to be hashed is the UTF-8 encoded string password+salt. 

Pseudo-code follows is directly from starbound's implementation.

```
//All inputs are UTF8 strings converted to bytes
//Client sends account from ClientConnect
account = UTF8ByteArray("testing")

//Server sends challenge and rounds from HandshakeChallenge
//Challenge is typically base64 encoded before being returned by the secure random number generator, but it is not base64 decoded by server or client
rounds = 5000
challenge = UTF8ByteArray("uAciRZkmwKUkek3krU+s2LTvPHE6v2P")

//Client sends its hash from HandshakeResponse
hash = UTF8ByteArray("test_password")

//From client side connect process
//passHash = Star::Auth::bcryptWithRounds(password, account + challenge->passwordSalt, challenge->passwordRounds);
salt = account+challenge

//From server side Star::Auth::bcryptValidate
if(rounds <= 99)
	throw exception("Not Enough Rounds")
	
hash = sha256(hash)
startTime = UnixTimestamp()

while (rounds > 0)
{
	if(UnixTimestamp() + 0.5 >= startTime)
		throw exception("Took too long")
	
	hash = sha256(hash+salt)
	rounds--
}

return Base64Encode(hash)
```

{% include connect-packet-header.html name="Connection Response" id="1" direction="server-to-client" %}
Assuming all previous packets have been sent, the client will receive a connection response. 
The first byte is a success flag. The following bytes are a client id, only applicable if the success flag is true, and then a rejection reason, only applicable if the success flag is false.
