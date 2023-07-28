## Lexer configuration

### Lexer specifiers

Currently lexer specifiers are only used to manage token precedence and associativity.

They are implemented in a similar fashion to bisons:

- You can use either `%left`, `%right`, `%nonassoc` or `%prec` to denote token associativity
- Token precedence increases with every group definition

For example:

```
%left t_plus, t_minus
%left t_mul
```

Would parse `5 + 5 * 5` as `plus(5, mul(5, 5))`.

However, if we slightly alter the grammar:

```
%left t_mul
%left t_plus
```

The same string would parse as `mul(5, plus(5, 5))`.

| Specifier name | Description                                                                       |
|----------------|-----------------------------------------------------------------------------------|
| `%left`        | Denotes that the token is `left-associative`                                      |
| `%right`       | Denotes that the token is `right-associative`                                     |
| `%nonassoc`    | Denotes that the token is `non associative` (`x %token y %token z` not permitted) |
| `%prec`        | Used to define precedence only, associativity conflicts remain unresolved         |

### Lexer rules

Lexer rules are the rules that describe how transformation from a stream of characters into a stream of tokens is performed. However, it is usually nothing more than mapping every token to its respective regular expression.

`Alien` uses the same approach here:
- Every token must define one and only one representation using a `regular expression`
- Every token must have an action associated with it. Actions are invoked after the lexer has parsed the token and must return a token object.

All regular expressions are than transformed into an `automaton`. This step guarantees deterministic and efficient tokenization.

You can find more information about regular expressions transformation [here](How%20It%20Works.md) (to be written).