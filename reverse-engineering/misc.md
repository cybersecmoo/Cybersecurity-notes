# Miscellaneous Stuff #

## Tools on Linux for Working with Windows Binaries ##

- Wine 
  - For running windows binaries
- dnSpy
  - For decompiling .NET binaries
- dotnetfiddle.net
  - For running .NET source code


## Obfuscation ##

- One method for obfuscating some assembly is to split the code up every few instructions, jumble the code up, and place mandatory `jmp` instructions in such that the actual flow remains the same
  - This will make static analysis a pain in the butt.
- To stymie linear-descent-based disassemblers, one can place absolute junk instructions (particularly long/variable length stuff) into the space below an unconditional jump (jump could be put in there manually) such that by the time it gets back to "real" instructions, it's been shifted such that it is offset from the instruction stream (e.g. disassembler thinks the second byte of an instruction is actually the first byte, or something)