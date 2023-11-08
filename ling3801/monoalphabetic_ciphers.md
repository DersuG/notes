# 02

## Terminology
- espionage communication
    - **cryptology:** hiding the message's meaning
        - **cryptography:** making and using with the key
            - **codes:** hiding words
            - **ciphers:** hiding letters
                - **substitution:** replacing existing letters
                - **transposition:** rearranging existing letters
        - **cryptanalysis:** cracking without the key
    - **steganology:** hiding the message itself
- **encryption/decryption:** for both codes and ciphers
- **enciphering/deciphering:** for ciphers
- **encoding/decoding:** for codes
## Conventions
- CIPHERTEXT IN CAPS
- plaintext in lowercase
- e.g. `the eagle has landed` -> `AAEGLNELSDHEAETHD`
## Monoalphabetic ciphers
- 1-to-1 mapping between plaintext and ciphertext
- **shift cipher:** ciphertext alphabet is shifted over (e.g. rot13)
- **exhaustive enumeration:** try all possibilities
- can shift until letter frequencies match english
- **keyword-based cipher:** ciphertext alphabet begins with a keyword
    - remaining letters appear in order
    - e.g. `SCARYDUKBEFGHIJLMNOPQTVWXYZ`
    - last few letters tend to match exactly (e.g. `wxyz` -> `WXYZ`)
- **Kama-Sutra cipher:** randomly pair letters (e.g. `ak` -> `KA`)
- **general monoalphabetic cipher:** just whatever
- all susceptible to frequency analysis
- **invariance:** the 1-to-1 mapping is invariant
    - guarantees there will be patterns in the ciphertext

## Kerckhoffs' principle
- systems should be secure even if enemies know everything about them
- when evaluating security, assume enemies know everything except the key and plaintext
