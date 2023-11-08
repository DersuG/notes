# Polygraphic Ciphers
- in monoalphabetic and polyalphabetic ciphers, 1 letter is enciphered at a time
- the Playfair cipher enciphers pairs of letters

## Playfair cipher
- key is 5x5 grid of letters of the alphabet (i and j collapsed)
    - order can be random or determined by key
    - grid starts with keyword, then remaining letters are filled in alphabetic order
- plaintext is split into letter pairs
    - letters in a pair must be unique (insert an `x` to separate pairs of the same letter)
    - add an `x` at the end if the last letter would be unpaired
    - `meetinthealleytonight` -> `me et in th ea lx le yt on ig ht`
- each pair is enciphered according to one of 3 rules:
    - **box rule:** letters of pair in different rows and different columns of key
        - the letters are corners of a rectangle
        - the pair enciphers to the other pair of corners (each letter becomes the other corner on its row)
    - **row rule:** letters of pair in same row of key
        - replace each letter with the letter immediately to its right (left when deciphering)
    - **column rule:** letters of pair in same column of key
        - replace each letter with the letter immediately below it (above when deciphering)

### Possible keys
- 25! possible grids, except many are not unique
- shifting a grid left/right/up/down gives an equivalent grid
- 5 possible column shifts and 5 possible row shifts = 25 classes of equivalent grids
- so only 24! possible grids

### Strengths
- each plaintext letter can be enciphered 5 different ways
- flatter unigram and trigram distributions

### Weaknesses
- bigram distribution is kind of preserved
- many invariants:
    - no letter stands for itself
    - XY and YX are always inverses
- knowing a plaintext/ciphertext pair is super useful

### Cryptanalysis
- tricks:
    - `e` frequently comes after `th`
    - if a ciphertext pair shares a letter with its plaintext pair, the 3 letters are in the same row/column
        - if `WZ` = `ew`, then `ewz` are in same row/column (?)
    - a letter cannot be enciphered to itself
    - repeated or flipped digraphs correspond to the same plaintext digraph
        - `er` and `re` are extremely common english digraphs
        - `th`, `er`/`re`, and `he` are common
    - find `the`:
        - should see 5 frequent ciphertext letters immediately after enciphered `th`
            - `{I|E|C|A|H}IC`
        - should see 5 frequent ciphertext letters immediately before enciphered `he`
            - `EI{C|H|T|I|P|B}`
        - common letters `TEICH` are probably in the same row
            - only possible row orientation is `TECHI` via row rule
    - the bottom parts of the grid are in alphabetical order
    - trigraphs:
        - `g` is usually before `ht`
        - 

### Cribs
- dragging:
    - crib plaintext could start on a digraph boundary or be 1 letter off
        - `re ga rd to [me sx sa ge] tw o`
        - `pe ry ou r[m es sa ge]`
- testing a placement:
    - look for flipped digraphs:
        - `SM KZ [ZI] BP [IZ] IO VD`
        - `?a tx [ta] ck [at] da wn`
        - 
