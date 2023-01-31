## REVERSING

- compiling a source code in windows results in a PE object file (portable executable)while compiling it in a linux results in a ELF object file (linux executable file).
Static analysis.
- static analysis is a method of analysing a binary without running it. (includes main functions and comparitively easier)obfuscations are now also applied in static analysis which makes the code deformed.
- Dynamic analysis is a method to analyse a binary while running the program.(doesnt include main functions and becomes harder to reverse it ).
- ".text" section is where the executable machine code is located.
- ".data" section is where global variables are stored.
- ".rdata" section is the place where global constants and external functions are stored.
####### assembly

- operands types -- immediate value,register,memory .
- memory operands are expressed as [] and a TYPE PTR is used  .like BYTE,WORD,QWORD,DWORD for 1bytes,2 bytes,4bytes,8 bytes.
1. mov QWORD PTR[rdi],rsi  --assign the value of rsi to the address pointed by rdi.
2. lea rsi,[rbx +8*rcx] -- substitute rbx + 8*rcx into rsi (address).
