# rce-runner

[![Deprecated](https://img.shields.io/badge/status-deprecated-red)](https://github.com/ToolKitHub/rce-runner)

> [!IMPORTANT]
> This repo is deprecated and has been moved to [https://github.com/ToolKitHub/rce-runner](https://github.com/ToolKitHub/rce-runner)


## Overview
This module is a command line application that reads code as a
json payload from stdin – compiles and runs the code – and writes
the result as json to stdout.
This is used by [rce-images](https://github.com/xosnrdev/rce-images) to run code on [cexaengine.com](https://cexaengine.com)
See the [overview](https://github.com/xosnrdev/carai) on how everything is connected.

## Prerequisites
rce-runner requires that the compiler / interpreter for the languages
you want to run is installed and is in PATH.

## Supported languages
- Assembly
- Ats
- Bash
- C
- Clojure
- Cobol
- CoffeeScript
- Cpp
- Crystal
- Csharp
- D
- Elixir
- Elm
- Erlang
- Fsharp
- Go
- Groovy
- Haskell
- Idris
- Java
- JavaScript
- Julia
- Kotlin
- Lua
- Mercury
- Nim
- Ocaml
- Perl
- Php
- Python
- Raku
- Ruby
- Rust
- SaC
- Scala
- Swift
- TypeScript

## Input (stdin)
The input is required to be a json object containing the properties `language`
and `files`. `language` must be a lowecase string matching one of the supported
languages. `files` must be an array with at least one object containing the
properties `name` and `content`. `name` is the name of the file and can include
forward slashes to create the file in a subdirectory relative to the base
directory. All files are written into the same base directory under the OS's
temp dir.

In addition, one may optionally provide the `stdin` and `command` properties to
provide stdin data to the running code and to run the code with a custom command.
See examples below.

## Output (stdout)
The output is a json object containing the properties `stdout`, `stderr` and
`error`. `stdout` and `stderr` is captured from the output of the ran code.
`error` is popuplated if there is a compiler / interpreter error.
Note that the rce-runner will exit with a non-zero code if invalid input is
given or if the files cannot be written to disk (permissions, disk space, etc).
No json will be written to stdout in those cases. Otherwise the exit code is 0.

## Examples

### Simple example
##### Input
```javascript
{
  "language": "python",
  "files": [
    {
      "name": "main.py",
      "content": "print(42)"
    }
  ]
}
```

##### Output
```javascript
{
  "stdout": "42\n",
  "stderr": "",
  "error": ""
}
```

### Read from stdin
##### Input
```javascript
{
  "language": "python",
  "stdin": "42",
  "files": [
    {
      "name": "main.py",
      "content": "print(input('Number from stdin: '))"
    }
  ]
}
```

##### Output
```javascript
{
  "stdout": "Number from stdin: 42\n",
  "stderr": "",
  "error": ""
}
```

### Custom run command
##### Input
```javascript
{
  "language": "bash",
  "command": "bash main.sh 42",
  "files": [
    {
      "name": "main.sh",
      "content": "echo Number from arg: $1"
    }
  ]
}
```

##### Output
```javascript
{
  "stdout": "Number from arg: 42\n",
  "stderr": "",
  "error": ""
}
```
