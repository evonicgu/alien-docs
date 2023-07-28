## Configuration headers

Configuration headers contain all the metadata which affects the generation result.

The header format is represented by the following grammar:

```
header              = stmts;

stmts               = stmts stmt | ;

stmt                = option | code-block | '{' definitions '}' | specifier;

option              = '#' id-sequence '=' value;

id-sequence         = <identifier> | id-sequence '.' <identifier>;

value               = <number> | <string> | identifier | boolean;

code-block          = '%code' position-specifier '{' <code> '}'

position-specifier  = 'decl' | 'impl' | 'header' | 'content-decl public' | 'content-decl private' | 'content-impl';

definitions         = definition | definitions ',' definition;

definition          = identifier ['=' identifier];

specifier           = '%' identifier spec-sequence;

spec-sequence       = identifier | spec-sequence identifier;
```

### Header parts

Headers consist of the following parts:

| Name          | Description                                                                                                      | Example                                                                                                                        |
|---------------|------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| Option | Used to tune the generator                                                                                  | <pre>#generator.track_lines = true</pre>                                                                                                |
| Definition    | An enumeration containing all symbol names and their respective types                                            | <pre>{<br>    t_digit = digit_token,<br>    t_plus,<br>    t_minus<br>}<br></pre>                                               |
| Code block    | Piece of code to be placed in one of predefined locations inside of the generated code.                          | <pre>%code impl {<br>    int string_to_digit(const std::string& text) {<br>        return std::stoi(text);<br>    }<br>}</pre> |
| Specifier     | Specifiers are used to provide additional information to the generator (such as associativity, precedence, etc.) | <pre>%left plus minus<br>%left mul div<br>%right exp<br></pre>                                                                  |

#### Options

Options are used to tune the generator in different ways. Therefore, it is important to be familiar with them to generate efficient and easy-to-use analyzers.

A Full list of lexer and parser options is available [here](Full%20option%20list.md).

#### Definitions

Definitions are essentially name declarations for later use in rules (in `alien` every rule must have a name associated with it).

If rule type is not specified the default type is assigned by the rule below:

| Generator | Default type                      |
|-----------|-----------------------------------|
| Lexer     | `#generation.token_type`          |
| Parser    | `nullptr (nothing is returned)`   |

#### Code blocks

Code blocks are an easy way to customize the configured code by inserting user-provided pieces of code in specific places.

There are plenty of use cases for code blocks. One of the most common are:

1. Custom error function implementation
2. Including necessary headers
3. Declaring and defining utility functions for the analyzers

There are six possible code block locations:

- `%code headers` blocks are used at the top of the header file templates. Thus, it is convenient to include dependencies in these blocks.

- `%code decl` blocks are useful for providing function declarations. These blocks are used in the header file template prior to the class definitions.

- `%code impl` blocks provide function definitions. These blocks are used in the source file template prior to the class definitions.

- `%code content-decl public` blocks are useful to declare public methods on generated classes.

- `%code content-decl private` blocks are useful to declare private methods on generated classes.

- `%code content-impl` blocks provide method implementation for methods declared in `%code content-decl public` or `%code content-decl private` blocks.

It is important to note, that the code is copied verbatim to the output file, so tabulation is dependent on the location of the code.

#### Specifiers

Specifiers are designed to configure specific implementation details of lexer or parser generators. Thus, they can mean different things in lexer or parser configuration contexts.

Currently, specifiers are only used to configure token precedence and associativity (like in lex/flex).