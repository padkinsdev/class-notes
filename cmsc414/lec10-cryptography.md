# Cryptography

## Introduction
- Reminder: Confidentiality means that an attacker cannot guess with better than random probability what the text of a message is
- Cryptographic properties are usually framed as "games"
- Cryptographic schemes are usually allowed to leak the length of a message
- Some schemes are theoretically vulnerable but secure in practice, e.g. cryptographic schemes that would take longer than the heat death of the universe to break. Thus, in gamified representations of such schemes attackers are generally limited to polynomial runtime
- IND-CPA is a game in which an attacker tries to discern which of two plaintexts has been encrypted (and must do so without just looking at message length) with probability greater than 1/2 + gamma, where gamma is negligible
- "Negligible" in this case generally means exponentially small within the bounds of the algorithm's security parameters. Different algorithms have different security parameters, but 2^80 is a good rule of thumb
- An encryption scheme is thus IND-CPA secure if an attacker cannot win the game with probablility greater than 1/2 + gamma

## One-time pad
- One-time pad is a symmetric-key cryptographic scheme which prioritizes confidentiality
- The fundamental idea: 
    * Alice and Bob share a key
    * That key is XOR-ed with the plaintext to generate the ciphertext
    * Bob receives the ciphertext and XOR-s the ciphertext with the key to obtain the plaintext
- The symmetric key that Alice and Bob share has length n, where n is the length of the plaintext
- XOR is a fast operation, making the process of encrypting and decrypting trivial if one has the correct key
- The scheme is confidential because the key is randomly generated and the probabilities of the correct plaintext being one option or another is exactly 1/2

### Two-time pad
- This is a representation for n-time pad, in which the key is used two or more times in a row to encryt different messages
- This is unwise because the two ciphertexts can be XOR-ed together to cancel out the key ((M1 XOR K) XOR (M2 XOR K) = (M1 XOR M2))
- If the attacker knows the plaintext content of one of the messages, they can further XOR the result (M1 XOR M2) to to get the other plaintext
- Ultimately, one-time pads are impractical despite offering good security. They are used when one has a secure communication channel now but will not have one later
- Soviet spies used one-time pads to communicate with spies in the US, but eventually started reusing keys, leading to the VENONA cryptanalysis project to break the encryption

## Block ciphers
- Block ciphers are symmetric-key encryption schemes
- The fundamental idea is to encode and decode a fixed-size block of bits
- The inputs are a k-bit key and an n-bit input, and the output is an n-bit ciphertext
- Block ciphers exhibit correctness, efficiency, and security
    * Block ciphers are *correct* because the encryption algorithm E(M) is a permutation (bijective function) on n-bit strings. In other words, each input corresponds to a unique output
    * Block ciphers are *secure* because they act like a randomly chosen permutation from the set of all permutations on n-bit strings
    * Block ciphers are *efficient* because the encoding and decoding processes are fast (microseconds)
- Block ciphers are not a specific encryption scheme but rather a class of schemes with the same property (block encryption)
- Brute forcing block ciphers is generally very inefficient
- Most modern CPUs provide dedicated hardware to support block ciphers

### Data encryption standard (DES) and advanced encryption standard (AES)
- DES was designed in the late 1970s
- AES was made an industry standard in 2001 when computing became powerful enough to crack DES via brute force
- Block ciphers are not IND-CPA secure because each encryption result is deterministic, and thus an attacker can simply check if the ciphertext corresponding to a specific plaintext matches the ciphertext in question
- AES uses keys of size 128, 192, or 256, though nowadays 256 is the main size used
- Block ciphers like AES can only encrypt messages of a fixed size, which presents an issue at first glance. Intuitively, one may simply break the plaintext into chunks of the specified size (e.g. 128 bytes), which is called electronic code book (ECB) mode
- ECB mode is deterministic and thus not IND-CPA secure. Adding an initialization vector, which is an additional random bit string, to the encryption process, adds randomness and thus results in non-deterministic results based on the key and plaintext alone (i.e. plaintext block 1 XOR initialization vector -> encrypt the result to get the first block of ciphertext)
- XOR-ing the result of the first block of ciphertext with the second plantext before perfoming the encryption adds additional randomness, and is called cipher block chaining (CBC) mode
- Decrypting CBC mode consists of reversing the encryption process