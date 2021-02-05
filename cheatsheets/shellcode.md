# Shellcode

## Basics

- Compiler will add null bytes if the size of the value in a register is smaller than the register itself
  - So use the relevant sub-registers (`ax`, `al`, etc.) where necessary
- Can use `XOR [register], [register]` to generate a null byte without explicitly putting one in the shellcode

### Common Bad-Characters

- 0x00
- 0x0a (LF)
- 0x0d (CR - can be a bad character for things supporting windows)

### Some syscall codes (x86_64 Linux)

- `0x3c`: `sys_exit` - useful for learning/PoC shellcode
- `0xe7`: `sys_exit_group` - as above (though not really necessary for exit-shellcode)

## Generating Shellcode

1. Write a program in C which does what you want your shellcode to do
2. Compile it (with `-static` to preserve syscalls)
3. Disassemble it
4. Check out what's going on in there
5. Optimise the assembly
6. Adjust the assembly to remove bad characters
7. Adjust assembly to make addresses relative
8. Compile assembly
9. Objdump to grab opcodes.

### Making Addresses Relative
