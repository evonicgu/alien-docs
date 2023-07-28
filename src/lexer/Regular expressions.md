## What are regular expressions?

Regular expressions are a fairly standard way of describing string templates. They are heavily used for input validation, recognizing of patterns, searching, replacing and etc.

In lexer generators regular expressions are typically used to define the tokens of the programming language. This includes keywords, operators, literals and others. Here's an example of string literal regular expression:

```
\"[^\"]*\"
```

This regular expression matches any sequence of characters between two double quotes, allowing for escape characters and other special characters within the string.

There are many regular expression standards (PCRE, ECMAScript) that define what different expressions mean. However, not all of their features are implemented in `Alien`, so this page defines exactly which features are available and their syntax.

### Patterns

| Pattern     | Description                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------|
| `x`         | Match character 'x'                                                                                      |
| `rs`        | Match regular expression `r` followed by regular expression `s`                                          |
| `r\|s`      | Match `r` or `s`                                                                                         |
| `(r)`       | Match `r`, parenthesis are used to override precedence                                                   |
| `.`         | Match any character except `\n` (ascii-only mode) or `\R` (utf8 mode)                                                                                      |
| `[xyz]`     | Character class, matches 'x' or 'y' or 'z'                                                               |
| `[a-fS]`    | Character class, matches any character between 'a' and 'f' and 'S'                                       |
| `[^x-z]`    | Negative character class, matches any character except 'x', 'y', 'z'                                     |
| `r*`        | Zero or more occurrences of `r`, where `r` is a regular expression                                       |
| `r+`        | One or more occurrences of `r`                                                                           |
| `r?`        | Zero or one occurrence of `r`                                                                            |
| `r{n,m}`    | Range quantifier, matches anywhere between `n` and `m` occurrences of `r`, where `n` and `m` are numbers |
| `r{,m}`     | Matches anywhere between 0 and `m` occurrences of `r`                                                     |
| `r{n,}`     | Matches `n` or more occurrences of `r`                                                                   |
| `r{n}`      | Matches `r` exactly `n` times                                                                            |
| `<<<EOF>>>` | Matches end of file                                                                                      |

Regular expressions are grouped by precedence:

1. Parenthesis (`(r)`)
2. Quantifiers (`r+`, `r*`, `r?`, `r{n, m}`)
3. Concatenation (`rs`)
4. Alternation (`r|s`)

For example,

```
foo|bar*    <=> ((foo)|(ba(r*)))
```

```
foo|(bar)*  <=> ((foo)|((bar)*))
```

```
(foo|bar)*  <=> (((foo)|(bar))*)
```

### Escape sequences

Escape sequences are another useful feature provided by `Alien`. They allow to denote characters by special sequence or by their number, refer to whole character groups, use unicode properties and more.

| Escape sequence    | Description                                        |
|--------------------|----------------------------------------------------|
| `\h`               | Matches any horizontal whitespace character        |
| `\H`               | Negation of `\h`                                   |
| `\X`               | Matches any valid unicode character                |
| `\R`               | Matches any unicode newline character              |
| `\s`               | Matches any whitespace character                   |
| `\S`               | Negation of `\s`                                   |
| `\d`               | Matches any digit                                  |
| `\D`               | Negation of `\d`                                   |
| `\n`               | Matches newline character (ASCII 10)               |
| `\N`               | Matches any non-newline character                  |
| `\t`               | Matches a tab character (ASCII 9)                  |
| `\v`               | Matches any vertical whitespace character          |
| `\V`               | Negation of `\v`                                   |
| `\w`               | Match any word character                           |
| `\W`               | Negation of `\w`                                   |
| `\f`               | Matches a form-feed character (ASCII 12)           |
| `\r`               | Matches a carriage return (ASCII 13)               |
| `\0`               | Matches the NUL character (ASCII 0)                |
| `\u{n}` or `\U{n}` | Matches the character with the given hex value `n` |
| `\pX` or `\p{Xx}`  | Matches a unicode character with given property: http://www.fileformat.info/info/unicode/category/index.htm   |

Any other character after `\` is matched literally, e.g. `\*`, `\[` mean just `*`, `[`.