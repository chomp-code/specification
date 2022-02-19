# [chomp-code specification](./readme.md) > binary encoding

A chomp-code program can be described as a binary stream.

Unsigned 8-bit integers are encoded as follows:

| Bits     | Value |
| -------- | ----- |
| 00000000 | 0     |
| 11010111 | 215   |
| 11111111 | 255   |

The stream starts with an unsigned 8-bit integer specifying how many inputs are to be given to the program.

There is then an unsigned 8-bit integer specifying how many outputs are to be taken from the program.

The remainder of the stream is a repeating data structure describing an instruction and its arguments:

- An unsigned 8-bit integer specifying the instruction's opcode.
- The arguments to the instruction, in order; each is comprised of two unsigned 8-bit integers:
  - If the first is 255, the second is a constant value to use.
  - Otherwise, the two form a stack index (the first and second being the most and least significant bytes of an unsigned 16-bit integer respectively).

## Limitations

- It is not possible to give more than 255 inputs into a program.
- It is not possible to take more than 255 outputs from a program.
- It is not possible to reference stack indices beyond the first 65280.  A stack index overflow is expected to be detected and handled by the program generating the stream, most likely by refusing to generate the stream at all.  Note that outputs can still be taken from the top of the stack, even if arguments of new instructions cannot reference them.
