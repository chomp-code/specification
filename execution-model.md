# [chomp-code specification](./readme.md) > execution model

## Execution Lifecycle

A chomp-code program centers around an initially empty stack of unsigned 8-bit integers.  An index into this stack is relative to the bottom; for example, index 0 is at the bottom, index 1 is first from the bottom, and so on.

The inputs to the program are first pushed onto the stack; for example, the first argument is placed at index 0, the second at index 1, and so on.

Then, for each instruction in the program:

- The instruction is executed.  Each of its argument is either a constant included in the program itself, or a reference to an item in the stack, by index.
- The result(s) of the instruction are pushed onto the top of the stack, in order.  For example, if there are 4 items in the stack before the instruction is executed, and the instruction generates two results, the first is placed at index 4, and the second is placed at index 5.

Then, the outputs from the program are taken from the top of the stack.  For example, if the program has 2 outputs and 7 items in its stack, the first output is at index 5, and the second is at index 6.

## Invalid Programs

Programs meeting any of the following criteria are deemed invalid and further processing must not be attempted:

- The program accepts no input.
- The program produces no output.
- An instruction references a stack index which is not populated by the time it executes; for example:
  - The stack index refers to an output of the instruction which is being executed.
  - The stack index refers to an output of an instruction subsequent to that being executed.
  - The stack index is greater than that which will be populated by any instruction in the program.
- The stack has fewer items after all instructions are executed than it is to output.

## Dubious Programs

Programs meeting any of the following criteria are not invalid, but are likely to have been written incorrectly:

- An instruction has all-constant arguments.
- An input or a result of an instruction is not taken as an output, nor referenced by
- An input is taken as an output.

Execution environments must still execute programs meeting these criteria, but should remark on them if met.
