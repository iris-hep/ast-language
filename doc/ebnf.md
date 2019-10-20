# AST language grammar

## Introduction

A grammar definition for this AST language can be found below, in [extended Backus-Naur form (EBNF)](https://en.wikipedia.org/wiki/Extended_Backus-Naur_form). In particular, this definition follows [ISO/IEC 14977:1996](https://www.cl.cam.ac.uk/~mgk25/iso-14977.pdf), which is a problematic version of EBNF but the only official standard that I could find. Note that this is only valid for printable ASCII characters; the exact syntax used can be found in [syntax.lark](/ast_language/syntax.lark) (in Lark's variety of EBNF), and it is valid for any character set.

The syntactic primary is a `record`, which is either empty or holds one top-level AST node.

## EBNF specification

```ebnf

record = expression | {whitespace chracter} ;

expression = {whitespace character} node {whitespace character} ;

whitespace character =   " "
                       | ? ISO 6429 character Horizontal Tabulation ?
                       | ? ISO 6429 character Carriage Return ?
                       | ? ISO 6429 character Line Feed ? ;

node = atom | composite ;

atom = identifier | string literal | numeric literal ;

identifier = (letter | "_"), {alphanumeric character | "_"} ;

letter =   "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J" | "K" | "L"
         | "M" | "N" | "O" | "P" | "Q" | "R" | "S" | "T" | "U" | "V" | "W" | "X"
         | "Y" | "Z"
         | "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l"
         | "m" | "n" | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x"
         | "y" | "z" ;

alphanumeric character = letter | digit ;

digit = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;

string literal =   "'", {character - "'" | escape sequence}, "'"
                 | '"', {character - '"' | escape sequence}, '"' ;

escape sequence = "\", character ;

character = alphanumeric character | symbol | whitespace character ;

symbol =   "!" | '"' | "#" | "$" | "%" | "&" | "'" | "(" | ")" | "*" | "+" | ","
         | "-" | "." | "/" | ":" | ";" | "<" | "=" | ">" | "?" | "@" | "[" | "\"
         | "]" | "^" | "_" | "`" | "{" | "|" | "}" | "~" ;

numeric literal = [sign], (unsigned integer | unsigned integer, ".", [unsigned integer] | [unsigned integer], ".", unsigned integer),
                  [("E" | "e"), [sign], unsigned integer] ;

sign = "+" | "-" ;

unsigned integer = digit, {digit} ;

composite = "(", {whitespace character}, node type,
                 {whitespace character, {whitespace character}, node},
                 {whitespace character}, ")" ;

node type = letter, {letter} | operator symbol;

operator symbol = "*" | "+" | "-" | "/" ;

```
