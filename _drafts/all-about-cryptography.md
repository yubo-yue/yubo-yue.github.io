---
layout: post
title:  "All about micro service"
date: 2018-11-23 01:12:13
categories: Cryptograhpy, Cipher, Security, web security
---

## Introduction
After learning the "Cryptograhy, the Big Picture" lecture in pluralsight, I got basic understanding of cryptography. 

The material is very great. It is good for beginner like me hope to quickly get a whole picture of cryptography. And it is great basis for next move if you want to dive deeper. 

Cryptography is one of basic principles. You can see it was used almost everywhere, and the trend is growing and growing, never slow down. 

To think bigger and long-term, it is also good start to extend learning to broader scope which include but not least to:
- OpenID connect
- OAuth
- Kerbores


## Objectives

1. Basic grasping of cryptography to let me to discuss with others. the terms, the different technologies use scenario, weakness and strength. 
2. Try to take myself as consultant. Can give direction for any projects where need take cryptograhy into consideration.
3. Know what is related and know basic how.

## Ask questions
 
* What do you want to protect?
  * Encrypt the file
  * Encrypt the message
  * Encrypt the link
* Use cases
  * Key exchange
  * Encryption
  * Authentication (digital signature) against man-in-the-middle attack.

* What the strength and weakness of asymmetric and symmetric algorithm?
  * Symmetric
    * Pros
      * Fast
      * Secure
    * Cons
      * Need another way to securely transmit key.
      * Key management can be difficult.
      * Doesn't provide non-repudiation.
  * Asymetric
* Key transmission is other problem to be solved.

* Possible use case for symmetric algorithm?
  * file transfer/storage
  * VPN
  * Transaction data (3DES)


* What is SSL?
  * See this article first (SSL Certificate How-To)[http://www.tldp.org/HOWTO/SSL-Certificates-HOWTO/x64.html]


## CIA

Confidentiality, Integrity and Availability.

* Confidentialy - is about keeping thing private.
* Integrity - is about who you are and not be manipulated. 
* Availability - you get to thing when you want. 

## OAuth 2.0 and OpenID Connect
OAuth 2.0 is a delegation protocol, a mean of letting someone who control a resources allow a software application to access that resources on their behalf without impersonating them.

