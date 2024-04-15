# Final Review

## Misc.
- **side channel attack:** get info about a system through other means (e.g. timing)
- **stream cipher period:** how long it takes the key to repeat
- **autokey vegenere:** the plaintext is appended to the key to create the actual key

## Block ciphers
- DES
- AES
- **confusion:** each key element affects the whole ciphertext
- **diffusion:** each plaintext element affects the whole ciphertext

## DHM key exchange
1. Alice picks private key $a$
2. Bob picks private key $b$
3. Alice and Bob decide on a public modulus $N$ and base $g$
4. Alice sends public key $A=g^a \mod{N}$ to Bob
5. Bob sends public key $B=g^b \mod{N}$ to Alice
6. Alice computes the shared key $S=B^a \mod{N}$
7. Bob computes the shared key $S=A^b \mod{N}$

## Message signing
![[message_signing.png]]
