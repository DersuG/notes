# Brand X

## MA substitution cipher with shift
- shift after every letter
- not significantly more secure:
    - `the` has 26 different encipherments, with enough ciphertext, frequency analysis is still possible
    - double letters in ciphertext decipher to adjacent plaintext letters (eg. `II` becomes `ed`)

### Cryptanalysis

If we know the first letter, we can find it again (in this case, `F`):

```
FHYPAU AF DRAT
a12345 67
```

where the numbers denote shifts.

```
abcdefghijklmnopqrstuvwxyz
F1234567..................
```

becomes

```
abcdefghijklmnopqrstuvwxyz
7..................F123456
```

and now we know

```
FHYPAU AF DRAT
a12345 6t
```

## Stacking shifting MA ciphers
- stacking 2 just creates another one (still repeats after 26 letters)
- if the 2nd cipher only shifts when the first makes a full cycle, it repeats after every 26^2 (676) letters
    - stacking multiple of these gives 26^3, 26^4, 26^5, etc.
    - allowing the ciphers to be swapped increases the possibilities by n! times
