KCV
===

Key Colon Value 0.1.0

KCV is a plain-text data format, influenced by [JSON] and [TOML].
The goal is to reduce the syntax and semantics to a viable minimum.
Therefore, KCV represents a one-dimensional dictionary, where each
key maps to a list of atomic values. Nothing more.

KCV is released under the terms of the [Creative Commons CC0].


Example
-------

    singleValue: 42
    threeValues: "Hello" 3.14 yes
    spaceGalore:
       1  23   4
      56   7  89
    newline:no problem:no


Encoding
--------

KCV must be a valid [UTF-8] encoded document.

Whitespace characters are: space (U+0020), tab (U+0009),
line feed (U+000A), and carriage return (U+000D).


Items
-----

A KCV document contains an unordered set of zero or
more items. Each item starts with a key, followed by
an ordered list of zero or more values, until the next
key or the end of the document.

Keys are marked with a suffixed colon (U+003A).

Key names start with an ASCII letter, followed by zero
or more ASCII letters, ASCII digits, hyphens (U+002D),
periods (U+002E), and underscores (U+005F). Key names
are case-sensitive and must be unique per document.

Values can be Booleans, numbers, or strings.

Each value must be separated from a subsequent key or
value by at least one whitespace character. Otherwise,
whitespace is ignored around keys and values.


Booleans
--------

Boolean constants are lowercase `yes` and `no`.


Numbers
-------

Numbers can be decimal or hexadecimal.

Decimal numbers start with an integer,
followed by an optional fraction,
followed by an optional exponent.

An integer consists of one or more decimal digits.
Redundant leading zeroes are ignored. A negative
integer is prefixed with a hyphen-minus (U+002D).

A fraction starts with a period (U+002E),
followed by one or more decimal digits.

An exponent starts with a case-insensitive E
(U+0045 or U+0065), followed by an integer.

    positive: 42
    negative: -42
    fraction: 3.14
    exponent: 314e-2

Hexadecimal numbers start with a lowercase `0x`,
followed by one or more case-insensitive
hexadecimal digits.

    hexadecimal: 0xFFdd55

The range, precision, or implementation of numbers
is not dictated by KCV.


Strings
-------

Strings are enclosed in double quotes (U+0022).

    foo: "This is a string."

In a string, the backslash (U+005C) begins an escape sequence.
The following sequences are replaced with single characters:

    \" = U+0022 (double quote)
    \\ = U+005C (backslash)
    \t = U+0009 (tab)
    \n = U+000A (line feed)
    \r = U+000D (carriage return)

Any [Unicode scalar value] can be included with a hexadecimal
escape sequence. The format is either `\uXXXX` or `\UXXXXXXXX`,
where X is a case-insensitive hex digit:

    \u1E9E     = U+1E9E  (sharp S)
    \U0001f603 = U+1F603 (smiling face)

Any other escape sequence is invalid. Except for escape sequences,
all correctly encoded characters must be preserved verbatim.
Incorrectly encoded characters must not be propagated.


[Creative Commons CC0]: https://creativecommons.org/publicdomain/zero/1.0/
[JSON]: https://json.org
[TOML]: https://github.com/toml-lang/toml
[Unicode scalar value]: https://unicode.org/glossary/#unicode_scalar_value
[UTF-8]: https://www.unicode.org/glossary/#UTF_8
