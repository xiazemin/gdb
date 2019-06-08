```
(gdb) disassemble main
Dump of assembler code for function main:
   0x0000000100000d10 <+0>:     push   %rbp
   0x0000000100000d11 <+1>:     mov    %rsp,%rbp
   0x0000000100000d14 <+4>:     sub    $0x20,%rsp
   0x0000000100000d18 <+8>:     lea    0x227(%rip),%rdi        # 0x100000f46
   0x0000000100000d1f <+15>:    movl   $0x0,-0x4(%rbp)
   0x0000000100000d26 <+22>:    callq  0x100000ede
   0x0000000100000d2b <+27>:    movl   $0x0,-0x8(%rbp)
   0x0000000100000d32 <+34>:    mov    %eax,-0x10(%rbp)
---Type <return> to continue, or q <return> to quit---q
Quit
(gdb) 

```



