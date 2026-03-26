# Message Authentication Codes (MACs) and Key Sharing

## Introduction
- A MAc is calculated as a hash function of the encoded text and the key used to encrypt the message. The function outputs a fixed-length tag on the message
- A secure MAC is essentialy unforgeable without the key. This is denoted as EU-CPA, or essential unforgeability under chosen plaintext attack
- A scheme is EU-CPA secure if for all polynomial time adversaries, the probability of winning is 0 or negligible
<!-- Using a fixed-length MAC protects against length extension attacks -->
- Using two hashes (message + key) prevents a length extension attack
- MACs provide integrity, meaning that an attacker cannot tamper with the message without being detected
- While MACs don't specify *who* sent the message, you at least know that it was someone with the secret key

## Authenticated Encryption (AE)
- A scheme that simultaneously guarantees confidentiality and integrity (and authenticity, depending on the threat model)
- AE can be implemented by combining encryption schemes that provide confidentiality with ones that provide integrity
- Notably, computing a MAC and then encrypting requires one to decrypt before they are able to discern whether the input has been tampered with, which is computationally expensive and can lead to side-channel attacks
- Reusing keys across encryption schemes can cause issues/interference. Thus, it is beneficial to use a different key per scheme instance

### TLS 1.0 "Lucky 13" attack
- TLS 1.0 used MAC-then-encrypt, and used AES-CBC mode
- The Lucky 13 attack abuses MAC-then-encrypt to read encrypted messages by guessing a byte of plaintext and changing the ciphertext accordingly
- The MAC would error, but variations in time to error can reveal if a guess was correct

### Side-channel attack
- A security exploit that targets the physical implementation of a system, such as timing, power consumption, electromagnetic leakage

### AEAD encryption
- Secondhand method for authenticated encryption
- Uses a scheme that is designed to provide confidentiality, integrity, and authenticity
- Provides both confidentiality and integrity over the plaintext and integrity over additional data
- Example: Galois Counter Mode, which is very fast and powerful but devastating if used improperly

## Pseudorandom number generators (PRNGs)
- Symmetric key encryption largely depends on secure generation of random keys/nonces/IVs
- Entropy: a measure of uncertainty. High entropy is necessary for cryptographically secure random numbers
- The best source of entropy is physical, like a circuit on a CPU. Unfortunately this is expensive and slow to generate
- PRNGs use a little bit of true randomness (a seed) to generate a great deal of pseudorandom output
- For an attacker who does not see the internal state of the generator, the PRNG output is indistinguishable from true randomness
- PRNGs display the properties of correctness (deterministic), efficiency, and security
- While a PRNG can definitionally not be truly random, they can be indistinguishable from random, which is represented by the fact that an attacker cannot predict future PRNG output with probability greater than 1/2 plus a negligible value
- A hypothetical design for a PRNG would be a block cipher (AES) in CTR mode

## Stream ciphers
- Another symmetric key encryption scheme
- The basic idea is to use a PRNG output as the key for a one-time pad
- Stream ciphers are IND-CPA secure as long as the PRNG output is secure
- In some stream ciphers, security is compromised if too much plaintext is encrypted. Specifically, if the AES-CTR counter wraps around keys will be reused
- One benefit of stream ciphers is the ability to decrypt a section of ciphertext without needing to decrypt the entire ciphertext

## Diffie-Hellman key exchange
- A scheme for exchanging secret keys publicly (i.e. with the assumption that communications are publicly accessible)
- Steps:
    - Generate `a`, a secret key
    - Calculate `g^a mod p`, the public key
    - g and a are public
    - Other participant calculates `g^b mod p`, where b is their secret key
    - Exchange `g^a` and `g^b`
    - The shared key is `g^(ab) mod p`
    - Note: `p` is a large prime number, and `g` is a generator
- This relies on the discrete logarithm problem, where given `g`, `p`, `g^a mod p`, for a random `a`, it is computationally hard to find `a`
- Diffie-Hellman makes the assumption that given `g`, `p`, `g^a mod p`, and `g^b mod p` for random `a`, `b`, no polynomial time attacker can distinguish between a random value `R` and `g^(ab) mod p`
- Thus for a random `a`, `b`, `R`, `g^(ab) mod p` looks identical to `R`