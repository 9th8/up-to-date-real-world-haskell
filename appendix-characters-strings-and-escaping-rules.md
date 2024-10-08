# Appendix. Characters, strings, and escaping rules

This appendix covers the escaping rules used to represent non-ASCII
characters in Haskell character and string literals. Haskell's escaping
rules follow the pattern established by the C programming language, but
expand considerably upon them.

## Writing character and string literals

A single character is surrounded by ASCII single quotes, `'`, and has
type `Char`.

``` screen
ghci> 'c'
'c'
ghci> :type 'c'
'c' :: Char
```

A string literal is surrounded by double quotes, `"`, and has type
`[Char]` (more often written as `String`).

``` screen
ghci> "a string literal"
"a string literal"
ghci> :type "a string literal"
"a string literal" :: [Char]
```

The double-quoted form of a string literal is just syntactic sugar for
list notation.

``` screen
ghci> ['a', ' ', 's', 't', 'r', 'i', 'n', 'g'] == "a string"
True
```

## International language support

Haskell uses Unicode internally for its `Char` data type. Since `String`
is just an alias for `[Char]`, a list of \~Char\~s, Unicode is also used
to represent strings.

Different Haskell implementations place limitations on the character
sets they can accept in source files. GHC allows source files to be
written in the UTF-8 encoding of Unicode, so in a source file, you can
use UTF-8 literals inside a character or string constant. Do be aware
that if you use UTF-8, other Haskell implementations may not be able to
parse your source files.

When you run the `ghci` interpreter interactively, it may not be able to
deal with international characters in character or string literals that
you enter at the keyboard.

:::: note
::: title
Note
:::

Note

Although Haskell represents characters and strings internally using
Unicode, there is no standardised way to do I/O on files that contain
Unicode data. Haskell's standard text I/O functions treat text as a
sequence of 8-bit characters, and do not perform any character set
conversion.

There exist third-party libraries that will convert between the many
different encodings used in files and Haskell's internal Unicode
representation.
::::

## Escaping text

Some characters must be escaped to be represented inside a character or
string literal. For example, a double quote character inside a string
literal must be escaped, or else it will be treated as the end of the
string.

### Single-character escape codes

Haskell uses essentially the same single-character escapes as the C
language and many other popular languages.

  Escape            Unicode   Character
  ----------------- --------- ---------------------
  `\0`{.verbatim}   U+0000    null character
  `\a`{.verbatim}   U+0007    alert
  `\b`{.verbatim}   U+0008    backspace
  `\f`{.verbatim}   U+000C    form feed
  `\n`{.verbatim}   U+000A    newline (line feed)
  `\r`{.verbatim}   U+000D    carriage return
  `\t`{.verbatim}   U+0009    horizontal tab
  `\v`{.verbatim}   U+000B    vertical tab
  `"`{.verbatim}   U+0022    double quote
  `\&`{.verbatim}   *n/a*     empty string
  `'`{.verbatim}   U+0027    single quote
  `\\`{.verbatim}   U+005C    backslash

  : Single-character escape codes

### Multiline string literals

To write a string literal that spans multiple lines, terminate one line
with a backslash, and resume the string with another backslash. An
arbitrary amount of whitespace (of any kind) can fill the gap between
the two backslashes.

``` haskell
"this is a 
\long string,
\ spanning multiple lines"
```

### ASCII control codes

Haskell recognises the escaped use of the standard two- and three-letter
abbreviations of ASCII control codes.

  Escape              Unicode   Meaning
  ------------------- --------- ---------------------------
  `\NUL`{.verbatim}   U+0000    null character
  `\SOH`{.verbatim}   U+0001    start of heading
  `\STX`{.verbatim}   U+0002    start of text
  `\ETX`{.verbatim}   U+0003    end of text
  `\EOT`{.verbatim}   U+0004    end of transmission
  `\ENQ`{.verbatim}   U+0005    enquiry
  `\ACK`{.verbatim}   U+0006    acknowledge
  `\BEL`{.verbatim}   U+0007    bell
  `\BS`{.verbatim}    U+0008    backspace
  `\HT`{.verbatim}    U+0009    horizontal tab
  `\LF`{.verbatim}    U+000A    line feed (newline)
  `\VT`{.verbatim}    U+000B    vertical tab
  `\FF`{.verbatim}    U+000C    form feed
  `\CR`{.verbatim}    U+000D    carriage return
  `\SO`{.verbatim}    U+000E    shift out
  `\SI`{.verbatim}    U+000F    shift in
  `\DLE`{.verbatim}   U+0010    data link escape
  `\DC1`{.verbatim}   U+0011    device control 1
  `\DC2`{.verbatim}   U+0012    device control 2
  `\DC3`{.verbatim}   U+0013    device control 3
  `\DC4`{.verbatim}   U+0014    device control 4
  `\NAK`{.verbatim}   U+0015    negative acknowledge
  `\SYN`{.verbatim}   U+0016    synchronous idle
  `\ETB`{.verbatim}   U+0017    end of transmission block
  `\CAN`{.verbatim}   U+0018    cancel
  `\EM`{.verbatim}    U+0019    end of medium
  `\SUB`{.verbatim}   U+001A    substitute
  `\ESC`{.verbatim}   U+001B    escape
  `\FS`{.verbatim}    U+001C    file separator
  `\GS`{.verbatim}    U+001D    group separator
  `\RS`{.verbatim}    U+001E    record separator
  `\US`{.verbatim}    U+001F    unit separator
  `\SP`{.verbatim}    U+0020    space
  `\DEL`{.verbatim}   U+007F    delete

  :  ASCII control code abbreviations

### Control-with-character escapes

Haskell recognises an alternate notation for control characters, which
represents the archaic effect of pressing the `control` key on a
keyboard and chording it with another key. These sequences begin with
the characters `\^`, followed by a symbol or uppercase letter.

  Escape                                      Unicode                 Meaning
  ------------------------------------------- ----------------------- ------------------
  `\^@`{.verbatim}                            U+0000                  null character
  `\^A`{.verbatim} through `\^Z`{.verbatim}   U+0001 through U+001A   control codes
  `\^[`{.verbatim}                            U+001B                  escape
  `\^\`{.verbatim}                            U+001C                  file separator
  `\^]`{.verbatim}                            U+001D                  group separator
  `\^^`{.verbatim}                            U+001E                  record separator
  `\^_`{.verbatim}                            U+001F                  unit separator

  : Control-with-character escapes

### Numeric escapes

Haskell allows Unicode characters to be written using numeric escapes. A
decimal character begins with a digit, e.g. `\1234`. A hexadecimal
character begins with an `x`, e.g. `\xbeef`. An octal character begins
with an `o`, e.g. `\o1234`.

The maximum value of a numeric literal is `\1114111`, which may also be
written `\x10ffff` or `\o4177777`.

### The zero-width escape sequence

String literals can contain a zero-width escape sequence, written `\&`.
This is not a real character, as it represents the empty string.

``` screen
ghci> "\&"
""
ghci> "foo\&bar"
"foobar"
```

The purpose of this escape sequence is to make it possible to write a
numeric escape followed immediately by a regular ASCII digit.

``` screen
ghci> "\130\&11"
"\130\&11"
```

Because the empty escape sequence represents an empty string, it is not
legal in a character literal.
