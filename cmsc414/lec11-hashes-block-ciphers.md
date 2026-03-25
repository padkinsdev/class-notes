# Block Cipher Chaining and Cryptographic Hashes

## Cipher Block Chaining (CBC) mode
- Because CBC mode sequentially chains blocks when encrypting, encryption cannot be parallelized
- However, because decryption only requires the ciphertext, decryption can be parallelized
- AES-CBC only works on plaintext that is sized as a multiple of the block size, so padding is often necessary to normalize the message size
- The standard padding scheme involves padding with sequential bytes indicating the number of padding bytes, or an entire block of 0 repeated if no padding bytes are needed
- Reusing the initialization vector makes the encryption scheme deterministic, and two overlapping plaintexts will generate identical ciphertext up to the point that the contents of the plaintext messages diverge

## Counter (CTR) mode
- If an attacker doesn't know the key used in a block cipher, the output will look random
- CTR mode involves taking a single use number (nonce) and concatenating it with a counter indication how many blocks from the start one is, then using the encrypted result of that to XOR with the plaintext and generate the ciphertext
- Decryption involves simulating the encryption process (nonce + counter -> encrypt)
- Correspondingly, both encryption and decryption with CTR mode can be parallelized
- CTR mode also allows for truncating the ciphertext at the point where the plaintext ends
- AES-CTR is IND-CPA secure only if the nonce is not reused between messages. To be clear, the nonce *is* reused between blocks
- If the nonce is reused, and overlapping content in the plaintext of two ciphertexts will be identical. This is essentially equivalent to reusing a one-time pad
- Initialization vectors (IV) and nonces are basically the same thing, but the specific term used is dependent on the encryption mode (IV for CBC, nonce for CTR)
- CTR mode is better for performance due to the parallelization of encryption, but CBC is better for security
- The next IV/nonce is appended to the end of the plaintext before encryption, meaning that only the initial IV/nonce needs to be shared

## Ciphertext Feedback (CFB) mode
- Similar to CBC in that CBC mode first XORs the plaintext with the IV/last block of ciphertext and then encrypts the block, whereas CFB encrypts the IV on the first block and *then* XORs it with the plaintext to get the ciphertext (which is the encryption input for the next block)
- CFB mode doesn't require padding, but the consequences of leaking the IV can be a bit worse
- Only CFB decryption can be parallelized

## Block cipher overview
- Block ciphers are designed for confidentiality, but cannot guarantee integrity (that the message has not been tampered with) or authenticity (that the sender is who they claim to be). Thus an interloper (Mallory) can successfully tamper with the message
- Being able to approximately predict the input structure makes the ability to alter message integrity especially effective. Knowing what the plaintext is allows one to change specific characters in CTR mode
- Altering the message in CBC mode affects future blocks but this still doesn't mean that integrity and authenticity are ensured

## Cryptographic hashing
- A hash function `H(M)` maps an arbitrary message to a fixed-length output
- Hash functions are deterministic, and thus will produce the same output for a given input
- Hash functions offer preimage resistance (one-way-ness) such that for any given hash output y, it is infeasible to find an input x that produces the same output when hashed
- Hashes also offer collision resistance. This means taht it is infeasible to find a pair of inputs x != y such that H(x) = H(y)
- MD5 is very insecure, SHA1 is fairly insecure, and SHA2 is the current standard, though is vulnerable to length extension attacks. SHA3 is an alternate standard
- SHA2 and SHA3 come in a variety of hash lengths including 256, 384, and 512 bits
- **Length extension attack**: Given H(x) and the length of x (but not x itself), the attacker can create H(x || m) for any m of the attacker's choosing
- SHA2 is vulnerable to length extension attacks, but SHA3 is not
- Hashes provide integrity only based on the threat model
- Hashes are unkeyed functions, so anyone can generate a hash for any value they desire

<!-- ## Message Authentication Codes (MACs) -->