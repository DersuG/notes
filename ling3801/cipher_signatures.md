# Cipher Signatures

How to identify ciphers.

- playfair ciphertext doesn't have `J`
- playfair ciphertext doesn't have double-letter digraphs
- playfair ciphertext must be an even length
- transposition ciphertext matches english letter distribution without shifts
- vigenère ciphertext doesn't match english letter distribution unless split into buckets
- if bigram distribution is flat: probably polyalphabetic, playfair, or transposition
- double letters are much rarer in vigenère and transposition
- index of coincidence for unigram, bigram, and trigram frequencies
