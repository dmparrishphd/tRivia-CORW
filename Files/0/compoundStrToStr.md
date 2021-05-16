compoundStrToStr
================

Interpret a compound string.

Usage
-----

    compoundStrToStr(string, quote = "\x7f")

| Argument | Description                                  |
| -------: | :------------------------------------------- |
| `string` | a `character` string                         |
|  `quote` | a `character` string with a single character |

Value
-----

A `character` string interpreted by concatenating the strings
represented in the `string` argument.

Details
-------

The `quote` argument must satisfy `nchar(quote) == 1`.
The character specified by `quote` must not occur in `string`.
The `quote` argument is used in an intermediate step to
temporarily replace `\"` sequences.

Compound Strings
----------------

### BNF

Untested specification for the `string` argument:

    <quotes> ::= "
    <QUOTES> ::= \"
    <nul> ::= < the absence of any character >
    <Character>  ::= <QUOTES> | <nul> | < any printing character except <quotes> >
    <Characters> ::= <Character> | <Character> <Characters>
    <STRING> ::= <quotes> <Characters> <quotes>
    <string> ::= <nul> | <STRING>
    <s> ::= < the space character, U+0020 >
    <ss> ::= <s> | <s> <ss>
    <ss-opt> ::= <nul> | <ss>
    <continuation> ::= <ss> <STRING>
    <continuations> ::= <nul> | <continuation> | <continuation> <continuations>
    <compound-string> ::= <ss-opt> <string> <continuations> <ss-opt>
    
### Examples

The following representations are as might be stored in a text file or as output by `cat`:

|                  `string` | `compoundStrToStr(string)` | Notes                              |
| ------------------------: | :------------------------- | :--------------------------------- |
|                           |                            | input and output are empty strings |
|                           |                            | input is spaces, output is empty string |
|                      `""` |                            | output is empty string             |
|                   `"" ""` |                            | outout is empty string             |
| `"\"Hello,"   "World!\""` | `"Hello, World!"`          |                                    |
|            `"\"\t\v\f\r"` | `"\t\v\f\r`                | `\"` is the _only_ "escape sequence" processed |


Code
----

["42"](https://github.com/dmparrishphd/tRivia/blob/master/Files/4/2/0/compoundStrToStr.R)
