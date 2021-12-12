---
layout: post
title:  Latency of TLS protocol
description: |
  The primary goal of TLS/SSL protocol(s) is to provide privacy and data integrity between two communication applications. A full TLS/SSL handshake demands at least two round trips in addition to TCP/IP connection setup.  

tags:
  - thoughts
  - latency
  - protocols
  - tls
---

# Latency of TLS protocol

## TLS protocol summary

The primary goal of TLS/SSL protocol(s) is to provide privacy and data integrity between two communication applications. The protocol is composed of two layers:
* TLS Record Protocol
* TLS Handshake Protocol

**TLS Record protocol**

* It encapsulates a high-level protocol data including TLS Handshake protocol.
* connection is private; symmetric cryptography is used for data encryption.
* connection is reliable; message integrity check.
* maintain a connection state. It specifies a compression algorithm, an encryption algorithm, and a message authentication code (MAC) algorithm.

The key used for data encryption is uniquely generated for each connection during handshake.

**TLS Handshake protocol**

It allows remote peers to authenticate each other and negotiate an encryption algorithm and keys:
* peer authentication based on public key cryptography, e.g. RSA
* negotiation of a shared secret is secure
* negotiation is reliable

A performance of cryptographic algorithms was a primary bottleneck couple of years ahead. Nowadays, hardware-assisted SSL terminators and modern CPU can perform over 1000 handshake per second on single CPU core. An overall TLS performance has been studies and improved independently by Tor project and Google.

## TLS Handshake procedure

A full TLS/SSL handshake demands at least two round trips in addition to TCP/IP connection setup.

![tls handshake procedure](/assets/images/2010-08-26-tls.svg)

**first round trip**
* ClientHello contains a highest supported TLS protocol, a random number, a list of supported cipher suites and compression methods.
* ServerHello contains chosen parameters for connection: protocol version, cipher suite, random number and compression methods.
* Server sends its Certificate (Cert) message.
* Server sends a ServerHelloDone (SHD) to indicate that handshake is over.

**second round trip**
* The client sends a ChangeCipherSpec (CCS) record to alter connection state.
* The finally client sends Finished message containing a hash and MAC over the previous handshake messages. If server fails to decrypt this message and validate hash/MAC then connection is terminated.
* And server sends, CCS record and Finished message.
* Connection is ready

A protocol has built-in shortcut in the handshake procedure. If the client persist a previous session id then it could be reused to map the cryptographic parameters previously negotiated, especially master secret. A random data attached to ClientHello and ServerHello messages should guarantee that the generated connection keys will be different than in the previous connection. In the RFCs, this type of handshake is called an abbreviated handshake.


## TLS False start

A false start [proposal](https://tools.ietf.org/html/draft-bmoeller-tls-falsestart-00) allows to minimize SSL handshake latency by one round-trip. The client sends application data before it has received and validated last Finished message from remote peer. There are two possible scenario:
* Both peer received and successfully validated "Finished" message. Hand- shake completed successfully.
* "Finished" message cannot be validated. Handshake is failed due to transmission error or attacker targets the connection.

![tls false start](/assets/images/2010-08-26-tls-false-start.svg)


## Certificate size

The sender follows up TCP/IP slow start algorithm while transferring Certificate chain. The initial congestion window (initcwnd is about 4K). If the certificate chain overflows the congestion window then extra round-trip is involved as the server waits for ACK from client.

## TLS record

Each record has signature and header, an record overhead about 25 to 40 byte. It is very important not to send a small packets with small records in them. OpenSSL build a record from each call to SSL write. Note if Nagle is disabled at TCP/IP it will send out packets immediately to minimize latency.

On another hand the OpenSSL would not decrypt/validate record until the whole record has been received. A transfer/receival of large records would involve extra round-trip before congestion window opens up. A record size of 1403 bytes for AES based cipher-suite has been proposed. However, GPRS interface has MTU of 1400 byte in contrast of 1500 for broadband/ethernet connection. Thus a record size should be around 1300 bytes.