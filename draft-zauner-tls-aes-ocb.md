---
title: AES-OCB (Offset Codebook Mode) Ciphersuites for Transport Layer Security (TLS)
abbrev: AES-OCB Ciphersuites
docname: draft-zauner-tls-aes-ocb-latest
date: 2016-01-01
category: std

ipr: #2640 2647
area: General
workgroup: TLS Working Group
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: A. Zauner
    name: Aaron Zauner
    organization: "lambda: resilient.systems"
    email: azet@azet.org

normative:
  RFC7253:
  RFC5246:
  RFC6066:
  RFC5116:
  RFC4279:
  RFC2119:
  RFC5288:
  RFC6066:
  RFC4279:
  RFC6347:
  draft-ietf-tls-chacha20-poly1305-04:

informative:
  RFC6655:
  AES:
    title: Specification for the Advanced Encryption Standard (AES)
    date: 2001-11-26
    author:
      org: National Institute of Standards and Technology
    seriesinfo:
      NIST: FIPS 197
  OCB:
    title: "OCB: A Block-Cipher Mode of Operation for Efficient Authenticated Encryption"
    author:
      - ins: P. Rogaway
      - ins: M. Bellare
      - ins: J. Black
    seriesinfo:
      CCS01: ACM Conference on Computer and Communications Security (CCS 2001), ACM Press, pp. 196-205
      date: 2001
  IAPM:
    title: "Encryption Modes with Almost Free Message Integrity"
    author:
      - ins: C. Jutla
    seriesinfo:
      EUROCRYPT01: Proc. Eurocrypt 2001, pp. 529-544
    date: 2001
  AELIMIT:
    title: Limits on Authenticated Encryption Use in TLS
    author:
      - ins: A. Luykx
      - ins: K. Paterson
    seriesinfo:
      date: 2016-03-08

--- abstract
This memo describes the use of the Advanced Encryption Standard (AES)
in the Offset Codebook Mode (OCB) of operation within Transport Layer Security (TLS)
and Datagram TLS (DTLS) to provide confidentiality and data origin authentication.
The AES-OCB algorithm is highly parallelizable, provable secure and can be
efficiently implemented in software and hardware providing high performance.
Furthermore, use of AES-OCB in TLS is exempt from former IPR claims by various parties.

--- middle
# Introduction
This document describes the use of the Advanced Encryption Standard (AES)
in the Offset Codebook Mode (OCB) of operation within Transport Layer Security (TLS)
and Datagram TLS (DTLS) to provide confidentiality and data origin authentication.
The AES-OCB algorithm is highly parallelizable, provable secure and can be
efficiently implemented in software and hardware providing high performance.

Furthermore OCB Mode {{OCB}} for AES {{AES}} provides a high performance, single-pass,
constant-time AEAD alternative to existing and deployed block-cipher modes without the 
need for special platform specific instructions.

Authenticated encryption, in addition to providing confidentiality for the plaintext
that is encrypted, provides a way to check its integrity and authenticity. Authenticated
Encryption with Associated Data, or AEAD {{RFC5116}}, adds the ability to check the
integrity and authenticity of some associated data that is not encrypted. This document
utilizes the AEAD facility within TLS 1.2 {{RFC5246}} and the AES-OCB-based AEAD
algorithms defined in {{RFC5116}} and {{RFC7253}}.

The ciphersuites defined in this document use ECDHE, DHE or Pre-Shared-Key (PSK) as their
key establishment mechanism; these ciphersuites can be used with DTLS {{RFC6347}}. Since
the abiltiy to use AEAD ciphers was introduced in DTLS version 1.2, the ciphersuites defined
in this document cannot be used with earlier versions of that protocol.

# Conventions Used in This Document
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in {{RFC2119}}.

# Forward-secret AES-OCB Ciphersuites {#fssuites}
The ciphersuites defined in this document are based on the AES-OCB
authenticated encryption with associated data (AEAD) algorithms
AEAD_AES_128_OCB_TAGLEN96 and AEAD_AES_256_OCB_TAGLEN96 described
in {{RFC7253}}. The following forward-secret ciphersuites are defined:

      CipherSuite TLS_DHE_RSA_WITH_AES_128_OCB = {TBD1, TBD1}
      CipherSuite TLS_DHE_RSA_WITH_AES_256_OCB = {TBD2, TBD2}
      CipherSuite TLS_ECDHE_RSA_WITH_AES_128_OCB = {TBD3, TBD3}
      CipherSuite TLS_ECDHE_RSA_WITH_AES_256_OCB = {TBD4, TBD4}
      CipherSuite TLS_ECDHE_ECDSA_WITH_AES_128_OCB = {TBD5, TBD5}
      CipherSuite TLS_ECDHE_ECDSA_WITH_AES_256_OCB = {TBD6, TBD6}

These ciphersuites make use of the AEAD capability in TLS 1.2 {{RFC5246}}.

Because this document makes use of an AEAD construct, use of HMAC truncation 
in TLS (as specified in {{RFC6066}}) has no effect on the ciphersuites defined
herein.

The "nonce" construction is identical to that of draft-ietf-tls-chacha20-poly1305-04:

AES-OCB requires a 96-bit nonce, which is formed as follows:

1. The 64-bit record sequence number is serialized as an 8-byte, big-endian value and padded on the left with four 0x00 bytes.
2. The padded sequence number is XORed with the client_write_IV (when the client is sending) or server_write_IV (when the server is sending).

In DTLS, the 64-bit seq_num is the 16-bit epoch concatenated with the 48-bit seq_num.

This nonce construction is different from the one used with AES-GCM
in TLS 1.2 but matches the scheme expected to be used in TLS 1.3.
The nonce is constructed from the record sequence number and shared
secret, both of which are known to the recipient.  The advantage is
that no per-record, explicit nonce need be transmitted, which saves
eight bytes per record and prevents implementations from mistakenly
using a random nonce.  Thus, in the terms of [RFC5246],
SecurityParameters.fixed_iv_length is twelve bytes and
SecurityParameters.record_iv_length is zero bytes.

These ciphersuites make use of the default TLS 1.2 Pseudorandom
Function (PRF), which uses HMAC with the SHA-256 hash function.
The ECDSA-ECDHE, RSA-ECDHE and RSA-DHE key exchanges are performed 
as defined in {{RFC5246}}.

# Pre-Shared-Key (PSK) AES-OCB Ciphersuites {#psksuites}
As in {{fssuites}}, these ciphersuites follow {{RFC7253}}.
The PSK, ECDHE_PSK and DHE_PSK key exchanges are performed as specified
in {{RFC4279}}. The following Pre-Shared-Key (PSK) ciphersuites are defined:

      CipherSuite TLS_PSK_WITH_AES_128_OCB = {TBD7, TBD7}
      CipherSuite TLS_PSK_WITH_AES_256_OCB = {TBD8, TBD8}
      CipherSuite TLS_DHE_PSK_WITH_AES_128_OCB = {TBD9, TBD9}
      CipherSuite TLS_DHE_PSK_WITH_AES_256_OCB = {TBD10, TBD10}
      CipherSuite TLS_ECDHE_PSK_WITH_AES_128_OCB = {TBD11, TBD11}
      CipherSuite TLS_ECDHE_PSK_WITH_AES_256_OCB = {TBD12, TBD12}

The "nonce" input to the AEAD algorithm is identical to the one defined in
{{fssuites}}. These ciphersuites make use of the default TLS 1.2 Pseudorandom
Function (PRF), which uses HMAC with the SHA-256 hash function.

# Applicable TLS Versions
These ciphersuites make use of the authenticated encryption with associated data
(AEAD) defined in TLS 1.2 {{RFC5288}}. Earlier versions of TLS do not have support
for AEAD; for instance, the TLSCiphertext structure does not have the "aead" option
in TLS 1.1. Consequently, these ciphersuites MUST NOT be negotiated in older versions
of TLS. Clients MUST NOT offer these cipher suites if they do not offer TLS 1.2 or
later. Servers which select an earlier version of TLS MUST NOT select one of these
ciphersuites. A client MUST treat the selection of these cipher suites in combination
with a version of TLS that does not support AEAD (i.e., TLS 1.1 or earlier) as an error
and generate a fatal 'illegal_parameter' TLS alert.

# Intellectual Property Rights
Historically Offset Codebook Mode has seen difficulty with implementation, deployment
and standardization because of pending patents and intellectual rights claims on OCB
itself. In preparation of this document all involved parties have declared they will
issue IPR statements exempting use of OCB Mode in TLS from these claims.
Specifically -- OCB Mode as described in this document for use in TLS -- is based, and
strongly influenced, by earlier work from Charanjit Jutla on {{IAPM}}.

## Resolved IPR Claims
The following parties have made IPR claims in the past:

  * US Patent No. 7,093,126 (Issued Aug 15, 2006) - Filed Apr 14, 2000. Inventor Name: Charanjit S. Jutla, Assignee: IBM
  * US Patent No. 6,963,976 (Issued Nov 8, 2005) - Filed Nov 3, 2000. Inventor Name: Charanjit S. Jutla, Assignee: IBM
  * US Patent No. 7,046,802 (Issued May 16, 2006) - Filed 30 Jul 2001. Inventor Name: Phillip W. Rogaway, Assignee: Rogaway Phillip W
  * US Patent No. 7,200,227 (Issued Apr 3, 2007) - Filed 18 Jul 2005. Inventor Name: Phillip Rogaway, Assignee: Phillip Rogaway
  * US Patent No. 7,949,129 (Issued May 24, 2011) - Filed 23 Mar 2007. Inventor Name: Phillip W. Rogaway, Assignee: Rogaway Phillip W

  Use of technology described by these patents, when used with TLS, has been explicitly
  exempted from any previous claims by the original authors and patent holders.

# IANA Considerations
IANA is requested to assign the values for the ciphersuites defined in {{fssuites}}
and {{psksuites}} from the TLS and DTLS Ciphersuite registries. IANA, please note
that the DTLS-OK column should be marked as "Y" for each of these algorithms.

# Security Considerations
The security considerations in {{RFC5246}} apply to this document as well. The
remainder of this section describes security considerations specific to the
ciphersuites described in this document.

## (Perfect) Forward Secrecy
With the exception of two Pre-Shared-Key (PSK) ciphersuites intended for use in
constrained environments and embedded devices (IoT), defined in {{psksuites}},
this document deals exclusively with ciphersuites that are inherently forward-secret.

## Static RSA Key-transport
No ciphersuite is defined in this document that makes use of RSA as Key-Transport.

## Nonce reuse
AES-OCB security requires that the "nonce" (number used once) is never reused.
The IV construction in {{fssuites}} is designed to prevent nonce reuse.
Specifically, if there is any error in the nonce construction implementation,
it will simply be non-interoperable with conforming implementations.

## Data volume limit under a single key
There is a limitation on the total number of bytes that can be
transmitted under one set of keys. For the AES-OCB ciphersuites,
implementations MUST NOT transmit more than 2^36 bytes encrypted
under a single key: they MUST rekey or close the connection before
2^36 bytes are reached. These limitations are based on limitations
introduced in the TLS 1.3 draft for AES-GCM, this document adheres to
the same constraints. A detailed analysis can be found in {{AELIMIT}}.

# Acknowledgements
This document borrows heavily from {{RFC5288}}, {{RFC6655}} and 
draft-ietf-tls-chacha20-poly1305-04.

The author would like to thank Martin Thomson for his suggested change on the client
negotiation paragraph, Nikos Mavrogiannopoulos and Peter Gutmann for the discussion on
PSK ciphersuites, Jack Lloyd for content on the clarification of the TLS Record IV length,
Samuel Neves for suggesting the data-limitation paragraph from the TLS 1.3 draft
and the TLS Working Group in general for feedback and discussion on this document.
