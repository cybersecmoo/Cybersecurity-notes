# General Assembly Stuff #

## Intel vs. AT&T Syntax ##

- Intel: `OP DEST, SRC`
- AT&T: `OP SRC, DEST`

## Registers ##

### x86 ###

- EAX (Further broken down into)
  - ax
    - ah
    - al
  - Often used for function return values
- EBX
  - bx
    - bh
    - bl
- ECX
  - Often used for counting
- EDX
- EIP 
  - Instruction Pointer
  - Always points to the next instruction
  - We don't modify this ourselves
- ESP 
  - Stack pointer
  - Points to the "top" (actually the bottom, in memory) of the stack.
- EBP
  - Base pointer
  - Points to the "base" (actually the top, in memory) of the stack
- EFLAGS
  - Comprises all the flags
  - One bit per flag
  - Combinations of flags can be set by various logical, bitwise, and arithmetic operations
  - The flags can then be used to drive control flow.
  - Flags:
    - ZF: Zero flag
    - OF: Overflow flag
    - SF: Sign flag
    - CF: Carry flag
  - For disassembling/RE/exploit dev, generally not necessary to memorise exactly which ops set which flags and in what ways. If needed, can always consult a manual for that.