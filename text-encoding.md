# [chomp-code specification](./readme.md) > binary encoding

A chomp-code program can be described as an ASCII-encoded text stream.  This will usually be identified by a file extension of "cct".

## Preprocessing

All characters including and following a "#" (ASCII code 35) until and excluding the next line feed (ASCII code 10) are comments and are ignored.

The only valid line separator is a line feed (ASCII code 10).  Lines which are empty or contain only spaces (ASCII code 32) are ignored (except for counting towards line numbers for error messages, etc.).  The file may end with blank lines, but this is not required.

## Identifiers

An identifier is a piece of text referring to a value which meets all of the following criteria:

- It is at least 1 character long.
- It is at most 256 characters long.
- Its first character is a lower case letter (ASCII codes 97 to 122, inclusive).
- Its last character is a lower case letter (ASCII codes 97 to 122, inclusive).
- All characters between the first and last are one of the following:
  - A lower case letter (ASCII codes 97 to 122, inclusive).
  - A digit (ASCII codes 48 to 57, inclusive).
  - An underscore (ASCII code 95).

## Structure

### Input Specification

The first (non-blank) line must be as follows:

- Any number of spaces (ASCII code 32).
- "inputs" (ASCII codes 105, 110, 112, 117, 116 and 115).
- Any number of spaces (ASCII code 32).
- A semicolon (ASCII code 58).
- Any number of spaces (ASCII code 32).
- One or more identifier(s) specifying the names of the inputs to the program, separated by at least one space (ASCII code 32).
- Any number of spaces (ASCII code 32).

### Output Specification

The second (non-blank) line must be as follows:

- Any number of spaces (ASCII code 32).
- "outputs" (ASCII codes 111, 117, 116, 112, 117, 116 and 115).
- Any number of spaces (ASCII code 32).
- A semicolon (ASCII code 58).
- Any number of spaces (ASCII code 32).
- One or more digits representing the number of outputs (ASCII codes 48 to 57, inclusive).
- Any number of spaces (ASCII code 32).

### Instructions

Each (non-blank) line until the end of the file then declares an instruction, as follows:

- Any number of spaces (ASCII code 32).
- An identifier for each result from the instruction, separated by at least one space (ASCII code 32).
- Any number of spaces (ASCII code 32).
- An equals sign (ASCII code 61).
- Any number of spaces (ASCII code 32).
- The name of an instruction.
- At least one space (ASCII code 32).
- For each argument, separated by at least one space (ASCII code 32):
  - If the argument is a constant, one or more digits representing its value (ASCII codes 48 to 57, inclusive).
  - Otherwise, an identifier.
- Any number of spaces (ASCII code 32).

## Invalid Streams

Streams meeting any of the following criteria are deemed invalid and further processing must not be attempted:

- Two or more inputs have the same identifier.
- Two or more results of an instruction have the same identifier.
- The result of an instruction has the same identifier as an input.
- The result of an instruction has the same identifier as the result of a previous instruction.
