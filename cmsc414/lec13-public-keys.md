# Public Key Cryptography

## High level review
- `g` is a generator of mod N if for `{1,2,3,...}`, `{g^1 mod N, g^2 mod N, g^3 mod N, ...}`
- The discrete log problem states that given `g` and `g^x mod N` it is infeasible to compute `x`

## Diffie-Hellman key exchange
- Alice and Bob pick a random a and b respectively and send each other `g^x mod N` where `x` is a or b respectively. Then Alice and Bob compute `g^ab mod N` independently
- Obviously this doesn't prevent an active intermediary from interposing between the two parties and relaying messages back and forth by performing key exchanges with the two legitimate parties. Alice and Bob don't know that they're talking to each other, they just assume so because they're talking to someone with a legitimate key

## Public key cryptography
- Symmetric key cryptography has the downside of requiring key exchanges, thus requiring both parties to be online to perform the exchange. Additionally, for one-to-many communications there must be many independent key exchanges
- Diffie-Hellman is resistant to evesdropping but not tampering
- One potential solution is the use of a trusted third party to perform pairwise key exchanges and validate sender identity. Unfortunately, this requires the third party to actually be trustworthy and not read, alter, or forge messages, as well as to not go offline
- The better solution is public key cryptography, which comprises three algorithms and outputs two keys, a secret and a public, based on a source of randomness and a maximum key length
- Public key pryptography is a randomized algorithm with a nondeterministic output
- The public key is available to everyone, whereas only one party knows the secret key
- Vanilla RSA is deterministic. In practice, RSA-PKCS is used instead, which adds a nonce to the message
- RSA is slower than symmetric-key cryptography, but maximizes correctness and security
- The encryption function approximates a trapdoor function in which inversion of the process cannot be done without access to the secret key
- Public key cryptography is like a block cipher in that only messages up to a fixed size can be performed. This, combined with the slow nature of public key encryption, leads to the use of hybrid encryption

## Hybrid encryption
- Hybrid encryption involves generating a symmetric key K, encrypting a message with K, then encrypting K itself with public key encryption. The encrypted message and the encyrpted K are concatenated and sent to the recipient, who uses their private key to decrypt K and then uses K to decrypt the message itself

## Digital signatures
- Digital signatures take in a secret key SK and a fixed-length message, and output a signature s
- This is a nondeterministic algorithm
- Verification of the signature involves using the public key PK, message, and signature to generate a yes/no evaluation of whether a signature is legitimate
- Digital signatures verify correctness and security
- Digital signatures *also* provide non-repudiation, meaning that once a party signs a message, they cannot claim to have not signed it

## Distributing public keys
- Public key cryptography is not immune to man-in-the-middle attacks
- The most widely used way to deal with this is to trust the key on first use, and be alert if the key changes. This requires a leap of faith in which one assumes that they are not being attacked from the start
- Another strategy is to use a certificate authority. This involves having a trusted signature which signs a message informing the user whether to trust keys that they do not already trust
- Ultimately, regardless of how one goes about doing so, implicit trust must be placed in *someone*
- Usually operating systems will come with a large set of trusted certificates, called a root key store, built in. These certificates are used to establish trust in new certificates
- If the root key store happens to have any malicious certificates, any keys that are trusted based on signatures from the malicious certificate are similarly suspect

## Certificates
- Revocation of a certificate may occur if the owner of a trusted certificate becomes compromised, giving access to their secret key
- Browsers will periodically pull or query updated lists of trusted certificates from certificate authorities
- Rapid response certificate revocation is essential to maintain user security

## Storing passwords
- If access to password storage (but not working memory) is obtained, it is important that the attacker be deterred or prvented from recovering the passwords themselves
- Plaintext password storage is unwise because it reveals passwords upon compromise
- Similarly, storing an encrypted password with the key makes recovery trivial
- Storing hashed passwords is unwise because some passwords are more popular than others and thus identical hashes can be mapped to these popular passwords (e.g. with a rainbow table)
- Rainbow tables facilitate the compact storage of many passwords via a reduction function that takes a hash's output as its input and outputs a potential password input
- The best option is to store salted hashes. This makes the hashes of identical passwords differ from one another. However, a dictionary attack for a single user can still be used
- To slow down the attacker further, a fast hash is used iteratively to produce a slow hash function