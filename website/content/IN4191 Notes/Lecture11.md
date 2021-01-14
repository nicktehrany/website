+++
date = "2021-01-14"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-14"
title = "Lecture 11: Certificates, Key Transport, and Key Agreement"
slug = "Lecture 11"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

## Certificates, Key Transport, and Key Agreement (Chapter 18)

What is key management?

- Key generation
- Key distribution
- Key storage
- Key change
- Key usage
- Key destruction

Requirements of key generation:

- secret
- unpredictable
- strong key

It is often desirable to frequently change the key in a cryptographic system.

Types of keys:

- **Static (or Long Term) Keys:** few hours to a few years
- **Ephemeral or Session (or Short Term) Keys** a few seconds or a day

### Certificates and Certificate Authority

We need a binding, linking a public key to an entity. This is achieved for physical tokens such as smart cards with biometrics or PINs, and otherwise we can use digital certificates.

This requires a trusted third party (TTP) or a certificate authority (CA).

The system of CAs and certificates is called the Public Key Infrastructure (PKI).

