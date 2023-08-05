## Actions

Each pattern in a rule has a corresponding action which is a code block. The pattern ends with first unescaped `:`, then the code block is parsed. 
If the action is `{}`, then no token is returned and the input is simply discarded.

```
(\pL|$|_)(\w|$|\')*: {
    return new identifier_token(gettext(), start, curr);
}
```

This example matches arbitrary unicode identifier and returns an identifier token which includes the identifier itself, start and end positions.

The action starts with a `{` and lasts until the balancing `}` is found, it may span across multiple lines. It is entirely possible to use `C-strings`, one-line, multiline comments and code blocks in the actions, `Alien` knows about this features and the code block would be parsed correctly.

## Code

Actions can contain arbitrary C++ code with some restrictions, see below. The code is placed at the end of the `lexer::lex()` method in a switch statement handling all the cases, thus, all the actions have access to the `lexer` class, which might sometimes be useful.

Actions may return a value (token), which must be of type `generation.token_type` pointer. Since there is usually no need in actual token text processing shortcuts, there are some shortcuts:

```
import: {}[import];
```

Is equivalent to 

```
import: { return new token_t(token_type::t_import, start, curr); };
```

It is also possible to use a macro to return a value (if `generation.macros` is set to true):

```
import: { return _t_import; };
```

In the actions you are allowed to call any functions including those that are defined inside of the lexer (via `%code content-<pos>`). One essential function is `lexer::gettext()`. This function reads the current token from the `lexeme` buffer and returns its string representation (`std::basic_string<int>` or `std::basic_string<char>` are constructed from pointers to the start and the end of the token).

