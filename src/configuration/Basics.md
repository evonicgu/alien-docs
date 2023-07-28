## How is alien configured?

All the configuration lies in a single file. Single source of truth is more convenient than the traditional approach involving separate files, since `alien` does not allow lexer-only or parser-only generation.

Configuration file consists of 2 larger parts, that in turn contain 2 smaller ones.

1. Lexer configuration
   1. Lexer header
   2. Lexer rules
2. Parser configuration
   1. Parser header
   2. Parser rules

```text
Lexer header

%%

Lexer rules

%%

Parser header

%%

Parser rules

%%
```