# Welcome to Alien Documentation!

## What is Alien?

Alien is a convenient compiler frontend generator. It is supposed to make life of compiler developers easier by doing all the hard work of creating and maintaining portable and fast lexers and parsers. It is heavily inspired by [lex](https://en.wikipedia.org/wiki/Lex_(software))/[yacc](https://en.wikipedia.org/wiki/Yacc), [flex](https://github.com/westes/flex)/[bison](https://www.gnu.org/software/bison/) and uses similar techniques as they do.

Alien can generate both lexers and parsers from single configuration file making it much easier for them to cooperate properly. The output files are designed in such manner, that it is easy to change any part of the system and flexibility of the settings allows to configure lexer and parser for your needs.

## Introduction

If you are familiar with compiler design you must have heard terms like `lexer` (`scanner`, `tokenizer`) and `parser`. They are one of the first compilation steps nearly all programming languages. So let's first determine what `lexer` and `parser` are.

### Terminology

- Lexer - program that takes a stream of characters as its input and produces a stream of tokens as its output
- Parser - program that takes a stream of tokens as its input and creates an Abstract Syntax Tree (AST)

Sometimes, people refer to lexers and parsers together as `compiler frontend`. However, this term might be unclear, because `compiler frontend` may include some other things, for example semantic analysis (typechecking, etc.).

### Lexers and parsers in real world

Let's start with something simple - a mathematical expression.

For example, `5 + 5 * 5`.

We can easily recognize the pattern and evaluate the expression (`5 + 5 * 5 = 30`), however the machine stores just a sequence of bytes, nothing more. Thus, there is the need to transform this sequence into something meaningful.

That's when lexers and parsers come in. They take raw text as input and generate AST from it, something much more convenient to work with programmatically.

Let's start with the lexer.

In our example lexer is used to break down the input string into smaller parts:
- Numbers
- Operators (`+` or `*`)

Thus, our expression turns into these tokens:

`DIGIT, OP_PLUS, DIGIT, OP_MUL, DIGIT`

It is important to note, that lexers used in compilers rarely tokenize the whole string at once, we'll cover that in later sections.

Then the parser creates an AST from the token stream returned by the lexer:

```text
        '+'

    /         \

DIGIT(5)      '*'

          /         \


      DIGIT(5)     DIGIT(5)
```

This AST can be evaluated or passed down to other compilation steps. In our case it was more than enough to understand what lexers and parsers actually do.