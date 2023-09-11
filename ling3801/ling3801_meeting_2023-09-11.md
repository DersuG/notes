# Polyalphabetic Ciphers

## Monoalphabetic ciphers are weak
- same mapping from plaintext to ciphertext
- use frequency analysis to exploit this invariant
## Polyalphabetic ciphers
- use multiple cipher alphabets
- have a system to switch between them

## Vigenère cipher
- use all possible shift ciphers
- 26x26 table for English
- simple version: start at a certain shift, and shift +1 after encoding a letter
- real version: use a keyword
    - use the shifted alphabet that starts with the first letter of the keyword
    - then use the next letter of the keyword
    - repeat the keyword as needed
    - decryption: find column of the letter in keyword, then find the ciphertext letter in that column

## Weaknesses of vigenère cipher
- the key loops
    - "the" can only be enciphered a certain # of ways
    - repeated triplets in ciphertext are possibly the word "the" that happened to be encoded using the same part of the key
- figuring out the key length (# of alphabets) lets you do frequency analysis on each of them
    - repeated letter sequences are possibly the same word encoded with the same part of the key
    - the distances between the sequences are some multiple of the key length
    - estimate key length by finding many different repeating letter sequences, and looking at the common factors of the spacings between each of them

```
24 | 1 2 3 4 6   8 12       24
36 | 1 2 3 4 6     12 18
42 | 1 2 3   6 7         21
```
