---
layout: post
title:  "All about Crytography"
date: 2018-11-23 01:12:13
categories: Cryptograhpy, Cipher, Security, web security
---

## Introduction
Cryptography is one of basic principles. You can see it was used almost everywhere, and the trend is growing and growing, never slow down. It deserves time and focus on it continuously. 

## Objectives

1. Basic grasping of cryptography to let me to discuss with others. the terms, the different technologies use scenario, weakness and strength. 
2. Try to take myself as consultant. Can give direction for any projects where need take cryptograhy into consideration.
3. Know what is related and know basic how.

## What is need for Network Security? (Goal)
  For secure communication, there are some basic goals of network security that should be achieved: 
CIA (Confidentiality, Integrity and Availability)

* Confidentialy - Maintain the secrecy of the message being transmitted over a network. Only the sender and the intended receiver should be able to understand and read the message, eavesdroppers should not be able to read or modify the contents of the message. 
* Integrity -  any message sent over network must reach the intended receiver w/o modification made to it. 
* Availability - Information created and stored by organization should be available all the time to authorized users, failing which the information ceases to be useful. 

## What are the principles of network security?
The principles of network security include confidentiality, integrity, availability, nonrepudiation, access control and authentication.

* Nonrepudiation: after a message has been sent and received, the sender and receiver should not be able to deny about the sending and receiving of the message, respectively. The receiver should be able to prove that the message has come from the intended sender and not from anyone else. In addition, the receiver should be able to prove that the received message's contents are the same as sent by the sender. 
* Access Control: the term "access" involves writing, reading, executing and modifying. Thus, access control determines and controls who can access what. 
* Authentication: is concerned with determining whom you are communicating with. 

## Define a network security attack?
A network security attack refers to an act of breaching the security or authentication routines of a network. 

## Explain passive attacks and active attacks?
Network security attacks can be classified into 2 categories passive and active attacks
* Passive attack: the attacker indulges in eavesdropping, that is, listening to a communication channel and monitoring the contents of message. 
  * Release of message contents:
  * Traffic analysis
* Active attack: an intruder either alters the original message or creates a fake message. 
  * Masquerade: In computer terms, 'masquerading' is said to happen when an entity impersonates another. In such attack, an unauthorized entities try to gain more priviledge than it is authorized for. 
  * Replay: This active attack involves capturing a copy of the message sent by the original reader and retransmitting it later to bring about an unauthorized result. 
  * Modification of messages: This attack involves making certain modifications to the captured message, or delaying or reordering the mssage to cause an unauthorized effect. 
  * DoS: This attack prevents the normal functioning or proper management of communication facilities. 

## Security mechanism and security service?

({{ site.url }}/assets/security.service)

({{ site.url }}/assets/security.mechanism)

## Briefly explain the model of network security?
To provide secured communication over the network, a general model of network security was created, which enhanced network security. The model contains various components, which are as follows:
* Message: This is the information that is to be transmitted over the network.
* Principal: These refer to communication nodes, one which transmits the message( sender) and the other receives it (receiver)
* Security-related transformation: This related to the transformations made to the information to be sent, so that it is unreadable to an intruder. 
* Secret information: This refers to the information that is shared between the two principals and used while applying the transformation at both the sender's and receiver's ends. For example, in case of encryption, the secret information can be a key used for encrypting and decrypting the message. 
* Secure message: This refers to the message obtained after applying a transformation. 
* Logical information channel:
* Trusted third party: this is an entity that may either be responsible for transmitting the secret information to the two trusted parties while protecting it from an attacker, or may be responsible for settling disputes regarding the authenticity of a message transmittion between two parties. 

({{ site.url }}/assets/Model.of.network.security.png)

According to this model, there are four basic tasks that are required for designing any security service. These tasks are:
* Design an algorithm to perform security-related transformation.
* Generating the secret information to be used with the algorithm.
* Developing some techniques for the sharing and distribution of the secret information.
* Selecting a protocol to be used by two parties. This protocol makes use of the secret information and security algorithm to achieve a specific security service. 

## Explain the term cryptography in brief
TBD

## Explain Symmetric-key and asymmetric-key encipherment
Traditionally, cryptography involves the use of the same key for encrypting or decrypting the messages (symmetric-key encipherment). However, modern cryptography involves the use of different keys for encryption and decryption (asymmetric-key encipherment)

## What is key management? also, explain the functions of key management

## Amazon's security/identity/Compliance and Cryptography & KPI?


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

## OAuth 2.0 and OpenID Connect
OAuth 2.0 is a delegation protocol, a mean of letting someone who control a resources allow a software application to access that resources on their behalf without impersonating them.

## References
* The "Cryptograhy, the Big Picture" lecture in pluralsight
* Express Learning: Cryptography and Network Security (oreilly)
