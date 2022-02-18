# chomp-code specification [![License](https://img.shields.io/github/license/chomp-code/specification.svg)](https://github.com/chomp-code/specification/blob/master/license) [![Renovate enabled](https://img.shields.io/badge/renovate-enabled-brightgreen.svg)](https://renovatebot.com/)

Chomp code is a binary stream describing a simple stack-based bytecode.

The bytecode itself describes a constant-time sequence of operations, but implementations may execute it in non-constant time.

## Types

### `u8`

An unsigned byte.

| Bits     | Value |
| -------- | ----- |
| 00000000 | 0     |
| 11010111 | 215   |
| 11111111 | 255   |

### `u32`

An unsigned 32-bit little-endian integer.

| Bits                             | Value      |
| -------------------------------- | ---------- |
| 00000000000000000000000000000000 | 0          |
| 10101101101001001011011111010111 | 3619136685 |
| 11111111111111111111111111111111 | 4294967295 |

## Structure

### Header

The stream starts with a `u8` specifying the number of `u8` arguments which must be given when executing the program.  These form the initial stack, in order (first = the bottom of the stack, last = the top of the stack).

## Operations

The rest of the stream is a list of operations which are executed in the order that they are declared.  The result of each operation is added to the top of the stack at the time of execution.  The value at the top of the stack when all operations have executed is the result of the running the program.  It is an error to execute a program resulting in an empty stack, or a stack containing 4294967041 or more items.

Each operation starts with a `u8` specifying the opcode, then a list of the arguments to that opcode, in order.

| Name              | Opcode | Parameter | Parameter | Parameter | Result             |
| ----------------- | ------ | --------- | --------- | --------- | ------------------ |
| Add               | 0      | 20        | 50        |           | 70                 |
|                   |        | 250       | 6         |           | Undefined behavior |
| ClampedAdd        | 1      | 20        | 50        |           | 70                 |
|                   |        | 250       | 6         |           | 255                |
| WrappedAdd        | 2      | 20        | 50        |           | 70                 |
|                   |        | 250       | 6         |           | 0                  |
| Subtract          | 3      | 50        | 20        |           | 30                 |
|                   |        | 40        | 41        |           | Undefined behavior |
| ClampedSubtract   | 4      | 50        | 20        |           | 30                 |
|                   |        | 40        | 41        |           | 0                  |
| WrappedSubtract   | 5      | 50        | 20        |           | 30                 |
|                   |        | 40        | 41        |           | 255                |
| AmplitudeModulate | 6      | 25        | 170       |           | 16                 |

It is an error to specify any other opcode.

Each argument is a `u32`:

| Argument   | Description                                               |
| ---------- | --------------------------------------------------------- |
| 0          | The value at the bottom of the stack.                     |
| 1          | The value second from the bottom of the stack.            |
| 2          | The value third from the bottom of the stack.             |
| ...        | (range of stack pointers)                                 |
| 4294967039 | The value 4294967038 levels from the bottom of the stack. |
| 4294967040 | Constant `u8` of 0.                                       |
| 4294967041 | Constant `u8` of 1.                                       |
| 4294967042 | Constant `u8` of 2.                                       |
| ...        | (range of `u8` constants)                                 |
| 4294967295 | Constant `u8` of 255.                                     |

It is an error to end the stream before all arguments to the final operation are fully specified.

It is an error to mismatch argument types, e.g. `Not 32` or `Add false, false`.
