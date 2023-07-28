## Lexer options

| Option name                         | Description                                                                        | Value type | Default value           | Required |
|-------------------------------------|------------------------------------------------------------------------------------|------------|----------|-------------------------|
| `generation.token_type`             | Base token type                                                                    | String     | `<default>` (generated) | false    |
| `generation.position_type`          | Position type                                                                      | String     | `<default>` (generated) | false    |
| `generation.macros`                 | Indicates if token-macros are generated (more on them later)                       | Boolean    | `false`                 | false    |
| `generation.enum_class`             | Indicated if `enum class` is used for generation instead of raw `enum`             | Boolean    | `true`                  | false    |
| `generation.custom_error`           | Indicates if custom error function implementation is provided                      | Boolean    | `false`                 | false    |
| `generation.noutf8`                 | Indicates if an ascii-only lexer is generated                                      | Boolean    | `false`                 | false    |
| `generation.track_lines`            | Indicates if line tracking code is generated                                       | Boolean    | `true`                  | false    |
| `generation.buffer_size`            | File buffer size in bytes                                                          | Integer    | `131072` (128 KB)       | false    |
| `generation.lexeme_size`            | Single lexeme buffer size in bytes                                                 | Integer    | `32768` (32 KB)         | false    |
| `generation.emit_stream`            | Indicates if default utf-8 stream implementation is generated                      | Boolean    | `true`                  | false    |
| `generation.namespace`              | Lexer namespace                                                                    | String     | `lexer`                 | false    |
| `generation.no_default_constructor` | No predefined lexer constructors are generated if set to true                      | Boolean    | `false`                 | false    |
| `generation.monomorphic`            | Indicates if lexer class is specialized (`Stream` is not a template parameter)     | Boolean    | `false`                 | false    |
| `generation.stream_type`            | `Stream` type for specialization (only if `generation.monomorphic` is set to true) | String     | -                       | true     |
| `general.guard_prefix`              | `#ifndef`-guard prefix (`_GEN_PARSER` or `_GEN_TOKEN` is appended)                 | String     | `""`                    | false    |
| `token.namespace`                   | User-defined token types namespace relative to `generation.namespace`              | String     | `""` (same namespace)   | false    |
| `generation.path_to_header`         | Path to parser header from parser source (only if `header_only` is not used)       | String     | `<default>` (generated) | false    |

## Parser options

| Option name                         | Description                                                       | Value type | Default value         | Required |
|-------------------------------------|-------------------------------------------------------------------|------------|-----------------------|----------|
| `generation.type`                   | Parser generation algorithm (can be `lalr`, `slr`, `clr`)         | String     | `lalr`                | false    |
| `general.start`                     | Start symbol name                                                 | String     | -                     | true     |
| `generation.symbol_type`            | Base symbol type                                                  | String     | -                     | true     |
| `symbol.namespace`                  | User-defined symbol namespace relative to `generation.namespace`  | String     | `""` (same namespace) | false    |
| `generation.namespace`              | Parser namespace                                                  | String     | `parser`              | false    |
| `generation.no_default_constructor` | No predefined lexer constructors are generated if set to true     | Boolean    | `false`               | false    |
| `generation.custom_error`           | Indicates if custom error function implementation is provided     | Boolean    | `false`               | false    |
| `generation.use_token_to_str`       | Indicates if `token_to_str()` is used in default error messages   | Boolean    | `false`               | false    |
| `generation.default_token_to_str`   | Indicates if default `token_to_str()` implementation is generated | Boolean    | `false`               | false    |
