## Contexts

Contexts are a way of using different lexer rulesets based on previous actions.

By default, all rules are placed inside of the `initial` context, but this rule placement behavior can be altered by wrapping the necessary rules inside of context literal:

```
...
[-+]?\d+: { BEGIN(after_literal); ...}

<after_literal>:
[a-zA-Z_$][a-zA-Z0-9_$]*: {/* */}
<after_literal>:

...
```

This example recognizes user-defined integer literals, e.g. `123kg`, `321s`.

Each context has its own set of rules (even `EOF` rule redefinition is possible) and each context compiles down to separate DFA used later by the lexing algorithm.

A macro named `BEGIN` is generated if there is more than one context to allow switching between them:

```c++
#define BEGIN(ctx) current_context = BEGIN_##ctx
```

Right after context macros are generated mapping contexts to numbers:

```c++
#define BEGIN_after_literal 1
#define BEGIN_initial 0
```

Context switching is implemented this way to guarantee context availability at compile time (program wouldn't compile if unknown context is provided).

