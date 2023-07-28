## Matching

When the generated tokenizer is run, it consumes characters and moves through automata states simultaneously. Whenever a match is found it is recorded and the search goes on.

The search stops when it is not possible to consume more characters (there is no valid transition from current automata state). On such condition the last recorded match is used.

Once the match is determined the text can be obtained via `gettext()` method. After the corresponding action is run the lexer continues to consume the input.

## Buffers

Two buffers are used for the tokenizing process - input buffer, lexeme buffer.

Input buffer stores the bytes read from the file. Its size can be changed by using `generation.buffer_size` option, however, the default buffer size (128K) is recommended.

Lexeme buffer stores bytes copied from input buffer on every automata transition. This buffer is necessary to avoid situations when the input buffer is refilled and previous data is lost.

### Input buffer

Input buffer is filled during the construction of the lexer (unless default constructors are disabled) and is refilled every time new characters are needed.

Increasing the size of the buffer will benefit the execution time (since refilling the buffer is rather expensive) at the expense of memory usage. Thus, choosing appropriate input buffer size is crucial to overall performance of the lexer.

### Lexeme buffer and unread characters

The lexeme buffer tends to have smaller size than the buffer size (since tokens are usually a few characters long). However, it is important for the lexeme buffer size to be big enough, because an exception is thrown whenever the token size exceeds the lexeme buffer size.

As described earlier the generated lexer can backtrack. This happens when the last match was recorded several characters before. Therefore, these characters are considered unread, copied to the beginning of the lexeme buffer and need to be consumed before new characters from the input buffer.


## Example

Here is a basic example involving aforementioned topics.

```
interface: {};
in: {};
ter: {};
```
The input is `inter`. As soon as the first two characters are recognized and copied over to the lexeme buffer, a match is recorded (`in` token was matched).

But the lexer does not stop there, it copies over the last three characters since it was still possible to match the `interface` token (which is a longer match). However, when the end of file is reached the lexer executes the action corresponding to the `in` token, and `ter` is copied to the beginning of the lexeme buffer as unread characters to be read first when `lex()` is called again.