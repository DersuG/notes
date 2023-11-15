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
- public keys allow others to encrypt messages for someone
- private keys allow someone to decrypt messages encrypted with their public key

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
- also works using public-private key
    - can use Goldwasser-Micali
- private key: $p$ and $q$
- public key: $X$, a random non-quadratic residual mod $N$, where $N=pq$
- message is converted to number $M$
- alice picks $U$ such that $UM$ is quadratic ($UM=a^2 \mod{N}$)
- both $U$ and $a$ are sent

### Attacking digital signatures
- eve has to find values $U$ and $a$ that validate using alice's public key
- easiest way is to have alice's private key
- there may be other ways :eye:
- chosen plaintext attack: eve chooses a message for alice to sign
- eve chooses a special message for which alice's signature reveals her private key
- careful choice of random $U$ can prevent this
- *Rabin's scheme*

## Summary of goldwasser-micali
- one-way function: factoring, telling quadratics from non-quadratics
- alice's trapdoor: Fermat's little theorem can distinguish quadratics if you know factors
- public key: modulus N, non-quadratic a
- private key: factors of N=pq
- bob sends alice a message: encode 0 as random quadratic, 1 as random non-quadratic
- alice reads the message: use Fermat and pq to find quadratics
- alice signs a message: find U to make message quadratic, send square root
- bob verifies alice's signature: quadratic signature, multiply by U, compare to original message

## Usage
- for sending messages:
    - use symmetric encryption (e.g. AES) on the message itself
    - use public-key encryption only to secure the message key
    - why?
        - public-key encryption is slower
        - public-key encryption is much easier to break than symmetric encryption for a given key size
        - public-key is only used on one small piece of totally random data
        - easier to send to multiple people
- for signatures:
    - compute hash of message
    - sign the hash
    - verify by computing the hash of the message and comparing to the signed hash

## Hashing
- like encryption that can't be decrypted
- usually output a block of bits (e.g. 256)
- **hash value** and **message digest:** the output
- want:
    - deterministic
    - resists collisions
    - great diffusion (big changes in output for small changes in input)
    - easy to compute
    - impractical to reverse

## Distributing keys
- use certificates

## Security certificates
- third party confirms a website's public key
- can confirm with multiple third parties
- certificate authorities
- usually use several different keys
    - root key is kept offline, signs certs for...
    - intermediate keys, which signs certs for...
    - individual sites

## Web of trust
- web of mutual trust, where people in your network act as third party certificate authorities for you and vice versa
- has never gone mainstream
