## Installation

To install and use `alien` you need:

- `C++` compiler with support for `C++17`
- CMake build system

The installation process is straightforward:

1. Clone the repository
   ```sh
   git clone https://github.com/evonicgu/alien && cd alien
   ```
2. Build the project with cmake
   
   All dependencies are downloaded by [CPM](https://github.com/cpm-cmake/CPM.cmake) so there is no need to download them manually

   ```sh
   cmake -DCMAKE_BUILD_TYPE=Release -G <your generator (Ninja, VS, etc.)> -S . -B ./cmake-build-release
   cmake --build ./cmake-build-release --target alien
   ```

## Usage

Creating an `alien` configuration might be a little time-consuming, so let's focus on using the executable.

`Alien` is essentially a cli tool and it is controlled by options supplied during the invocation from the command line. Alien currently makes use of these options:

| Name                   | Short name | Description                                                                                                  | Required                               | Default          |
|:------------------------:|:------------:|:--------------------------------------------------------------------------------------------------------------:|:----------------------------------------:|:------------------:|
| input                  | i          | Path to configuration file                                                                                   | Yes                                    | -                |
| output                 | -          | Path to source file output directory                                                                         | Required, if not in `header_only` mode | -                |
| header_output          | -          | Path to header file output directory                                                                         | Yes                                    | -                |
| verbose                | v          | Makes the error output verbose. Currently mainly used for debugging (showing exception reasons).             | No                                     | `false`          |
| header_only            | -          | Indicates if the generator is working in `header_only` mode (declarations and definitions are not separated) | No                                     | `false`          |
| help                   | h          | Prints help message                                                                                          | No                                     | -                |
| quiet                  | q          | Quiet mode (no warnings, recommended for `CI/CD` usage)                                                      | No                                     | `false`          |
| lexer_source_template  | -          | Path to lexer source template file                                                                           | No                                     | Default template |
| lexer_header_template  | -          | Path to lexer header template file                                                                           | No                                     | Default template |
| tokens_template        | -          | Path to tokens template file                                                                                 | No                                     | Default template |
| parser_source_template | -          | Path to parser sourcetemplate file                                                                           | No                                     | Default template |
| parser_header_template | -          | Path to parser header template file                                                                          | No                                     | Default template |

## Note on the `header_only` mode

If the `header_only` mode is enabled by settings the `header_only` cli option to true the generator creates a single output header file, which contains all the definitions and declarations (they are separated to reuse the default source and header file templates).

It is easier to use the `header_only` mode, since one header file is much easier to use than the header + the source file. However, it might be better not to use the `header_only` approach, because making any change to the configuration will trigger a recompilation (which may take some time for big lexers and parsers). Use this option very carefully.

## Templating

`Alien` comes with five default templates, each of them is a prototype of generated code.

The default templates shipped with `alien` have everything you need to generate a lexer and a parser. However, customization is also possible: you can modify the default template or create your own one.

It is also important to note, that templates are combined in a predefined way. It means that it is not possible to change the template rendering order, although each template's content is modifiable.

You can find more information about templating in a separate topic (not done yet) in this wiki.

