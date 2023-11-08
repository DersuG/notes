# Transposition Ciphers
- because the letters are reordered, not changed, frequency analysis should match english distribution without any shift

## Railfence
- zigzag the message over multiple rows
- ciphertext is the content of each row appended together

## Columnar transposition
- write message in grid
- fill empty space at end with garbage
- associate each column with one letter of the keyword, in order
- rearrange the columns by sorting the keyword alphabetically
- repeated keyword letters are ok, just preserve relative order
- ciphertext is the combined text of the columns, top to bottom, left to right
- cryptanalysis:
  - use divisors of ciphertext length to guess grid size
  - fill columns with ciphertext
  - find digraphs/trigraphs in the same row (e.g. `qu`, `the`, etc.)
  - in english, ~40% of letters are vowels, check that the vowels per row make sense to narrow down likely grid sizes

### Systematic analysis
- find cribs (e.g. `THE`)
- make a table of compatible columns
    - find rows with only 1 `T`, `H`, and `E`
    - check that rows in that column order are plausible

### Weaknesses
- short messages easy to break
- vulnerable to digraph frequency counting
- cribs with rare letters
- partial solutions are easily extended
- 
