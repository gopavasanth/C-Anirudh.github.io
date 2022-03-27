---
title: Notes - Pretty Good Privacy (PGP)
date: 2020-06-25 13:50:00 +0530
categories: [Academic Notes, Information Security]
tags: [pgp, s-mime, email-security, notes]
---

> The following notes have been made with reference to the book [Cryptography and Network Security: Principles and Practice, 5th Edition by William Stallings](https://www.pearson.com/us/higher-education/product/Stallings-Cryptography-and-Network-Security-Principles-and-Practice-5th-Edition/9780136097044.html). Content has been quoted from the book. All credits to the author.
{: .prompt-info }

- With the increased reliance on email, there grows a demand for authentication and confidentiality services. Two such schemes that are used widely include: Pretty Good Privacy (PGP) and S/MIME.

## Pretty Good Privacy
- Created by Phil Zimmermann
- Provides confidentiality and authentication service that can be used for electronic mail and file storage applications
- Notations that will be used:

![Notations](/assets/img/posts/email-security/PGP-notation-1.png "Notations")
![Notations](/assets/img/posts/email-security/PGP-notation-2.png "Notations")

### Operational Description
- The actual operation of PGP consists of four services: **authentication**, **confidentiality**, **compression**, and **e-mail compatibility**

#### Authentication

This image illustrates the digital signature service provided by PGP.

![Authentication Service](/assets/img/posts/email-security/PGP-authentication.png "Authentication Service")

- The sequence is as follows:
    - The sender creates a message.
    - SHA-1 is used to generate a 160-bit hash code of the message.
    - The hash code is encrypted with RSA using the sender’s private key, and the result is prepended to the message.
    - The receiver uses RSA with the sender’s public key to decrypt and recover the hash code.
    - The receiver generates a new hash code for the message and compares it with the decrypted hash code. If the two match, the message is accepted as authentic.

- The combination of SHA-1 and RSA provides an effective digital signature scheme. Because of the strength of RSA, the recipient is assured that only the possessor of the matching private key can generate the signature. Because of the
strength of SHA-1, the recipient is assured that no one else could generate a new message that matches the hash code and, hence, the signature of the original message.
- As an alternative, signatures can be generated using DSS/SHA-1.
- Although signatures normally are found attached to the message or file that they sign, this is not always the case: Detached signatures are supported. A detached signature may be stored and transmitted separately from the message it signs.

#### Confidentiality

This image illustrates the confidentiality service provided by PGP.

![Confidentiality Service](/assets/img/posts/email-security/PGP-confidentiality.png "Confidentiality Service")

- The sequence is as follows:
    - The sender generates a message and a random 128-bit number to be used as a session key for this message only (one time key).
    - The message is encrypted using CAST-128 (or IDEA or 3DES) with the session key.
    - The session key is encrypted with RSA using the recipient’s public key and is prepended to the message.
    - The receiver uses RSA with its private key to decrypt and recover the session key.
    - The session key is used to decrypt the message.

- As always, one must address the problem of key distribution. In PGP, each symmetric key is used only once. That is, a new key is generated as a random 128-bit number for each message. Thus, although this is referred to in the documentation as a session key, it is in reality a one-time key. Because it is to be used only once, the session key is bound to the message and transmitted with it. To protect the key, it is encrypted with the receiver’s public key.

#### Confidentiality and Authentication

This image illustrates how both service can be used for the same message.

![Authentication and Confidentiality Service](/assets/img/posts/email-security/PGP-auth-and-confidentiality.png "Authentication and Confidentiality Service")

- First, a signature is generated for the plaintext message and prepended to the message. Then the plaintext message plus signature is encrypted using CAST-128 (or IDEA or 3DES), and the session key is encrypted using RSA (or ElGamal).

- In summary, when both services are used, the sender first signs the message with its own private key, then encrypts the message with a session key, and finally encrypts the session key with the recipient’s public key.

#### Compression

- As a default, PGP compresses the message after applying the signature but before encryption. This has the benefit of saving space both for e-mail transmission and for file storage.

- The placement of the compression algorithm, indicated by Z for compression and Z^–1 for decompression in the above Figures, is critical.

- The signature is generated before compression for two reasons:
    - It is preferable to sign an uncompressed message so that one can store only the uncompressed message together with the signature for future verification.If one signed a compressed document, then it would be necessary either to store a compressed version of the message for later verification or to recompress the message when verification is required.
    - Even if one were willing to generate dynamically a recompressed message for verification, PGP’s compression algorithm presents a difficulty. The algorithm is not deterministic; various implementations of the algorithm achieve different tradeoffs in running speed versus compression ratio and, as a result, produce different compressed forms. However, these different compression algorithms are interoperable because any version of the algorithm can correctly decompress the output of any other version. Applying the hash function and signature after compression would constrain all PGP implementations to the same version of the compression algorithm.

- Message encryption is applied after compression to strengthen cryptographic security. Because the compressed message has less redundancy than the original plaintext, cryptanalysis is more difficult.

- The compression algorithm used is ZIP.

#### E-mail Compatibility

- Many electronic mail systems only permit the use of blocks consisting of ASCII text. To accommodate this restriction, PGP provides the service of converting the raw 8-bit binary stream to a stream of printable ASCII characters.
- The scheme used for this purpose is `radix-64 conversion`. Each group of three octets of binary data is mapped into four ASCII characters. This format also appends a CRC to detect transmission errors.
- One noteworthy aspect of the radix-64 algorithm is that it blindly converts the input stream to radix-64 format regardless of content, even if the input happens to be ASCII text.

![Transmission and Reception of PGP Messages](/assets/img/posts/email-security/PGP-transmission-reception.png "Transmission and Reception of PGP Messages")

### Summary of PGP Services

![Summary of PGP Services](/assets/img/posts/email-security/PGP-services-summary.png "Summary of PGP Services")
