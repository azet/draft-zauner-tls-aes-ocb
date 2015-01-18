---
title: AES-OCB (Offset Codebook Mode) Ciphersuites for Transport Layer Security (TLS)
abbrev: AES-OCB Ciphersuites
docname: draft-ietf-tls-aes-ocb-latest
date: 2015-01-18
category: std

ipr: #XXX/TODO: IPRs 560, 1591, 1682, 1683
area: General
workgroup: TLS Working Group
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: A. Zauner
    name: Aaron Zauner
    organization: Independent
    email: azet@azet.org

normative:
  RFC5246:
  RFC6066:
  RFC5116:
  RFC4279:
  RFC2119:

informative:
  RFC7253:
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
      CCS01: ACM Conference on Computer and Communications Security (CCS ’01), ACM Press, pp. 196-205
    date: 2001


--- abstract
This memo describes the use of the Advanced Encryption Standard (AES)
in the Offset Codebook Mode (OCB) of operation within Transport Layer Security
and Datagram TLS (DTLS) to provide confidentiality and data origin authentication.
The AES-OCB algorithm is highly parallelizable, provable secure and can be
efficiently implemented in software and hardware providing high performance.

--- middle
# Some text

that follows more text
actually.

