# Fuzzing/Intro to Cryptography

## Fuzzing
- Again, an automated process involving generating and entering random inputs for a program
- Fuzzing is easy and cheap to implement, but don't tend to be structured properly, thus potentially not generating meaningful results
- During fuzzing, the program is monitored for errors, e.g. segfaults or stalls
- Fuzzing inputs are usually file-based or network-based

### Enhancements to regular fuzzing
- **Mutation-based fuzzing**: Take a (meaningful) seed input and mutate it e.g. by flipping bits, checking at each mutation step if it causes program errors. Anomalies can be random or structured
- Mutation based fuzzing is limited by the context of the original input
- **Generation-based fuzzing**: Start with an input specification and generate inputs that follow that specification. Should give better results than random fuzzing
- Code coverage is a metric for determining how much code is being executed. It doesn't actually determine runtime behavior, simply determines *whetehr* it's executed
    * **Line/block coverage**: How many lines of source code have been executed?
    * **Branch coverage**: How many of the total branches in the code have been taken?
    * **Path coverage**: How many of the possible paths in the code have been taken?
- Code coverage allows for comparison of fuzzer effectiveness
- Even full code coverage does not guarantee the finding of bugs
- **Grey-box fuzzing**: Runs mutated inputs on a program and measures code coverage. A genetic evolution algorithm is used in which mutated inputs that result in greater code coverage are added to the corpus of inputs

## Cryptography

### Introduction and terminology
- The common cast of characters in cryptographic scenarios is Alice, Bob, Eve, and Mallory. Mallory is, predictably, the villain
- The cryptographic principles are confidentiality (only the desired parties can read the message), integrity (bad actors cannot alter the message), and authenticity (messages actually come from the parties they are said to come from)
- The basic building block of a cryptographic algorithm is a key. Algorithms can be symmetric (both parties use the same key) or asymmetric (each party has a public key and a secret key)
- **Kerkhoff's Principle**: Related to Shannon's Maxim. Assume that the attacker understands how the system works. Cryptosystems should remain secure even when the attackers know the internal details of the system (except the private key). Additionally, the system should be designed such that leaked keys can be easily changed
- Confidentiality is akin to locking a box which only the desired recipient can unlock. In reality, the "box" is an encryption algorithm, and the "key" is the decryption algorithm
- Integrity is akin to adding a seal to an envelope so that the recipient can see if the letter was opened or altered based on whether the seal was broken. Encryption schemes provide integrity by adding a tag or signature to the message

### Symmetric-key encryption
- These schemes provide confidentiality
- Both parties use the same secret key to encrypt messages
- It is reasonable to assume that all data is represented as bitstrings before and after encryption
- Symmetric schemes have 3 algorithms: key generation, encryption, and decryption
- Encryption and decryption algorithms should be fast
- In the context of symmetric schemes, confidentiality means that regardless of what the adversary already knows (e.g. that each message starts with "Dear Bob"), they should not be able to gain *additional* knowledge. In other words, the ciphertext should not give the adversary any additonal information about the plaintext