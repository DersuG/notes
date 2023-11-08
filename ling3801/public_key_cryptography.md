# Public Key Cryptography

## Diffie-Hellman secret sharing
- (read slides)
- $A = f(a)$
- $k = A^b\mod{N}$
- weaknesses:
    - how do you know the receiver is who they claim to be?

## Public key cryptography
- alice and bob use each others' public keys to encrypt messages for each other
- alice doesn't need to be online to negotiate a new secret
- identities are persistent (NSA can't pretend to be alice unless they've been doing so from the start)
- the math can be used for other tools (e.g. digital signatures)
- many different public key systems exist
    - each one based on a (practically) one-way mathematical function

## Goldwasser-Micali
- use quadratic residuals (perfect squares mod n)
- $n^2\mod{N}$
- alice can tell residuals from non-residuals
    - suppose $N = pq$
    - then $x$ is a quadratic residual of $pq$ iff:
        - $x^{(p-1)/2} = 1 \mod{p}$
        - and
        - $x^{(q-1)/2} = 1 \mod{q}$
- how does Bob encipher?
    - generate a quadratic residual
    - send it
    - to generate a non-residual:
        - $a^2x \mod{N}$ for some random $a$ and where $x$ is the non-residual from Alice's key
            - $x$ is not a perfect square, thus neither is $a^2x$
- Eve can't decipher:
    - only way to tell perfect residuals from non-residuals is by knowing $p$ and $q$
    - no easy way to get $p$ and $q$ from $N$ (prime factorization)

### Guessed-plaintext attack
- Eve guesses/discovers the plaintext, uses Alice's key to encipher it

## Digital signatures
- prove you wrote a given message
- nobody can forge messages from you

