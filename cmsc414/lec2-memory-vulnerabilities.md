# Memory Based Vulnerabilities

### Memory addresses
- In a 32 bit system addresses are 32 bits, and can be represented by 8 heax bits
- esp, ebp, eip store addresses. eip is the instruction pointer, ebp is the frame pointer
- x86 uses little endian representations, so the least significant byte is stored first

### Buffer overflows
- If given a certain buffer but no bounding on the amount of data that can be written, one may simply write more data than would fit in the buffer to overwrite other data. This can be done to overwrite variables, overwrite return addresses, etc. Overwriting the return address is called stack smashing
- Notably, stack smashing or changing the return address, requires an understanding of the current structure of the stack in order to know *where* to direct the return address. In other words, you need to know what the address of your desired (malicious) code is in order to overflow the buffer in a way that sets the correct return pointer (eip)
- One option for mitigating the issue of not knowing where things are in the stack is to simply load the malicious code into the rest of the stack space, giving access to the full address space available to the hijacked program
- `nop` (0x90) is the "no operation" instruction, instructing the operating system to move to the next instruction. Stacking `nop` instructions allows you to "sled" or cascade forward to the malicious shellcode. Thus, one may simply write a stack of `nop` instructions and concatenate the malicious shell code, with the overwritten eip (return address) being a guess that should land among the nops

### printf
- printf uses a format string plus any number of argumants designating content to format the string with, e.g. `printf("x: %d, y: %d, z: %d", x, y, z)`
- printf can be abused to print out stack contents

### Integer conversion error
- In cases where type conversions (e.g. signed to unsigned integer) are implicitly performed, the type conversion may be used to convert a negative number (singed int representation) to a positive one (unsigned int representation)

### Off by one
- A simple mistake (`< vs <=`) in bounding can be used to access data beyond what the program writer intended