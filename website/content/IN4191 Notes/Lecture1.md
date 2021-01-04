+++
date = "2021-01-04"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-04"
title = "Lecture 1"
slug = "Lecture 1"
type = "notes"
katex = "true"
markup = "mmark"
+++

#### What is Security?

Security means:
**C**onfidentiality: Access to  systems or dataa is limited to authorised parties.  
**I**ntegrity: WHen you receive data, you get the "right" data.  
**A**vailability: The system or data is there when you want it.  

#### Objectives of Cryptography

    - Message authentication: valid message?
    - Data origin authentication: valid sender?
    - Enitity authentication: authenticate each other for communication

Kerkhoff's principle: The adversary knows all details about the crypto system except the private key.

$$Enc_k$$: plaintext to ciphertext  
$$Dec_k$$: ciphertext to plaitnext  
Encryption key: $$k$$  
Decryption key: $$k'$$  

if $$k=k'$$, symmetric key or private key encryption  
if $$k \neq k'$$, asymmetric key or public key encryption

#### Information Theoretic Security

Entropy:

$$-\sum_{i=1}^np_i*log_2p_i$$
