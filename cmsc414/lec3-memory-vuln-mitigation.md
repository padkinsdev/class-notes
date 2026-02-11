# Memory Vulnerability Mitigation

### Memory safe languages
- Programming languages that include a combination of compile-time and runtime checks that prevent memory errors from occurring, e.g. checking bounds, prevent undefined memory access. E.g. Java, Python, C#, Python, Rust
- Memory safe languages are believed to run slightly slower, e.g. in Java the garbage collector may add a 10-100 ms delay at any time. However, **on modern hardware the performance difference is usually insignificant**, with the exception of some embedded systems, operating systems, games
- C's improved performance is not a direct result of it having security issues, and memory safe languages like Go and Rust tend to run in comparable time
- Unfortunately, legacy code is largely written in C, and it's easier to just build on existing code than to rewrite the whole stack. Thus, newer code tends to be written in the same language as the legacy code

### Writing memory safe code
- Defensive programming: *Always add checks in your code!* E.g. check that a pointer is not null before dereferencing it, even if you're sure it's not going to be null
- Use safe libraries and functions that check bounds
- These are largely dependent on programmer discipline or tools that check the program for vulnerabilities
- Code review (have someone check your code)
- PenTesting
- Runtime checks, e.g. automatic bounds checking that crashes the program if the check fails
- Bug finding tools, e.g. static analysis via heuristics and fuzz testing with random inputs

#### Exploit mitigations
- **These are not foolproof, but do make common exploits harder**
- Compiler and runtime defenses

##### Non-executable pages
- Make pages executable or writable, but not both. E.g. the code section of the stack is executable but not writable, and the rest of the stack/heap/data is writable but not executable
- W^X
- Data Execution Prevention (DEP, used by Windows)
- No-execute bit
- *Issue: Non-executable pages don't prevent leveraging existing code in memory for exploit purposes. A good example of this is poisoned dependencies, or even using extraneous code that's loaded as part of a larger library (e.g. `<stdio.h>`)*

###### Return-to-libc: Overwrite the return pointer to jump to a standard c library function
- x86 function calls involve a prologue and epilogue. If you can rewrite the return address location in the function epilogue (which involves automatically restoring the function pointer to its location prior to the function call), you can attempt to jump to a standard C library function such as the C `system()` function, which is in the standard library
- Knowing *where* to jump to in memory requires additional exploits revealing the structure of memory and where different functions are located

###### Return-oriented programming: Patch together an exploit using pieces of code that are already in memory
- TODO: Learn more about return oriented programming
- ROP Gadget: A smell set of instructions that already exist in memory. They usually end in a `ret` instruction, and are usually not full functions
- If you chain gadgets together, you can produce meaningful results
- ROP compilers can automate the process of creating a ROP chain based on a target binary and desired malicious code
- If the code base is big enough (i.e. imports enough libraries) it can be reliably predicted that there will be enough ROP gadgets to string together any code you want

### Overview
- The general attack pattern is to find a memory safety vulnerability, write malicious code at a known address, overwrite the RIP with the address of the desired code, return from the current function to the desired code, and begin executing the malicious code
- Making each of these steps more difficult or impossible provides protection

### Stack canaries
- Adding a stack canary usually forces the attacker to overwrite it
- Generate a random secret value during runtime and save it to the canary storage. In the function epilogue, check the value in the canary storage location. If it has changed, there's a possibility that there is an attacker
- The overhead for using a stack canary is relatively minimal