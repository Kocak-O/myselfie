Copyright (c) the Selfie Project authors. All rights reserved. Please see the AUTHORS file for details. Use of this source code is governed by a BSD license that can be found in the LICENSE file.

Selfie is a project of the Computational Systems Group at the Department of Computer Sciences of the University of Salzburg in Austria. For further information and code please refer to:

selfie.cs.uni-salzburg.at

This is the grammar of the C Star (C\*) programming language.

C\* is a tiny subset of the programming language C. C\* features global variable declarations with optional initialization as well as procedures with parameters and local variables. C\* has five statements (assignment, while loop, if-then-else, procedure call, and return) and standard arithmetic (`+`, `-`, `*`, `/`, `%`) and comparison (`==`, `!=`, `<`, `>`, `<=`, `>=`) operators over variables and procedure calls as well as integer, character, and string literals. C\* includes the unary `*` operator for dereferencing pointers hence the name but excludes data types other than `uint64_t` and `uint64_t*`, bitwise and Boolean operators, and many other features. The C\* grammar is LL(1) with 7 keywords and 22 symbols. Whitespace as well as single-line (`//`) and multi-line (`/*` to `*/`) comments are ignored.

C\* Keywords: `uint64_t`, `void`, `sizeof`, `if`, `else`, `while`, `return`

C\* Symbols: `integer`, `character`, `string`, `identifier`, `,`, `;`, `(`, `)`, `{`, `}`, `+`, `-`, `*`, `/`, `%`, `=`, `==`, `!=`, `<`, `>`, `<=`, `>=`, `...`

with:

```
integer    = digit { digit } | "0x" { hex } .

character  = "'" printable_character "'" .

string     = """ { printable_character } """ .

identifier = letter { letter | digit | "_" } .
```

and:

```
digit  = "0" | ... | "9" .

hex	= "0" | ... | "9" | "A" | ... | "F" .

letter = "a" | ... | "z" | "A" | ... | "Z" .
```

C\* Grammar:

```
cstar             = { type identifier { array } |
                      [ "=" [ cast ] [ "-" ] ( integer | character ) ] ";" |
                    ( "void" | type ) identifier procedure } .

type              = "uint64_t" [ "*" ] | "struct".

cast              = "(" type ")" .

procedure         = "(" [ variable { "," variable } [ "," "..." ] ] ")" ( ";" |
                    "{" { variable ";" } { statement } "}" ) .

variable          = type identifier .

statement         = ( [ "*" ] identifier | "*" "(" and_or ")" | identifier { array } ) "=" and_or ";" |
                    call ";" | while | for | if | return ";" .

call              = identifier "(" [ and_or { "," and_or } ] ")" .

and_or		  = expression
		    [ ( "&" ) | ( "|" ) expression ] .

expression        = shift
                    [ ( "==" | "!=" | "<" | ">" | "<=" | ">=" ) shift ] .

shift		  = simple_expression { ( "<<" | ">>" ) simple_expression } .

array		  = "[" and_or "]"

struct		  = "{" { variable ";" } "};"

simple_expression = term { ( "+" | "-" ) term } .

term              = factor { ( "*" | "/" | "%" ) factor } .


factor            = [ cast ] [ "-" ] [ "*" ] [ "~" ]
                    ( integer | character | string |
                      identifier { array } | call | "(" and_or ")" ) .

while             = "while" "(" and_or ")"
                      ( statement | "{" { statement } "}" ) .

for               = "for" "(" statement ";" and_or ";" and_or ")"
                      ( statement | "{" { statement } "}" ) .

if                = "if" "(" and_or ")"
                      ( statement | "{" { statement } "}" )
                    [ "else"
                      ( statement | "{" { statement } "}" ) ] .

return            = "return" [ and_or ] .
```
