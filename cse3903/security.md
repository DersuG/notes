# Security
- **confidentiality:** non-authorized users don't have access
- **integrity:** accuracy/correctness of data
- **availability:** no downtime
- **authenticity:** agents are who they claim they are
- **non-repudiation:** a party to a transaction can't later deny it

## Attack methods
- target people (social engineering)
    - phishing
    - baiting (e.g. click & install, sussy flash drive, etc.)
- target software (exploits)
    - unpatched OS, browser, programs
    - buffer overflow
    - code injection and cross-site scripting
- target channel (mitm, etc.)
    - eavesdropping
    - masquerading (data tampering, replay, etc.)

## AES
- Advanced encryption standard (2001)
- replaced DES (1977)
- block size always 128 bits (4x4 bytes)
- key size is 128, 192, or 256 bits
- multi-step algorithm, many rounds

### Limitation of fixed block size
- what do when message is longer than block size?
- can't reuse same E for each block
    - danger: frequency analysis vulnerability
- solution: initialization vector

### Initialization vector
- add a random block to start
- combine adjacent blocks to make ciphertext block
    - many combination strategies (modes)
- propagates randomness or something idk
![[security_aes_initialization_vector.png]]

## Hashing
- **salt:** user-specific data added to passwords before they're hashed
- **pepper:** hard-coded data added to all passwords before they're hashed

## Performance
- symmetric key cryptography is faster than public key cryptography
- optimization for encryption (RSA)
    - make fresh symmetric key, k
    - use symmetric algorithm to encrypt m
    - use recipient's public key to encrypt k
- optimization for digital signatures
    - calculate digest for m (always short)
    - use sender's private key to encrypt digest

## TLS 1.3
- certificate authority
    - connects public key to identity
- client:
    - get server's public key
    - make new (symmetric) session key
    - sends this key to server (encrypted with public key)
    - (so using public key cryptography to bootstrap symmetric key cryptography)

