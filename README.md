# MarkScript — the markdown-native bytecode VM, in ONE file

> Your documentation is your program.

MarkScript is a markdown-native scripting runtime: `#` opens a **domain**,
`##` a **routine**, `>` dispatches an **intent**, tables are **typed data**,
fenced blocks are **code or data**. Markdown has no syntax errors — the only
errors are runtime errors. This repository is the entire language:
**`markscript.kn` (353 KB, 11 modules, raw Kain amalgamation).** One file.
That is the whole repo.

## Run it in 30 seconds

You need the [Kain](https://github.com/kainlang/kain) compiler (`kain.exe`
on PATH), then:

```bat
kain.exe build markscript.kn --target llvm -o mks.exe
```

Write `hello.md`:

```markdown
# Hello

## prove_it_works

```markscript
let n = 1
while n <= 5:
    print(n)
    n = n + 1
```
```

```bat
mks.exe run hello.md
```

Subcommands: `run` (default), `check` (compile-only), `disasm` (bytecode
dump), `repl`, `eval`, `jit`, `doc`, `pipe`. `mks.exe --help` lists them all.

## What this is

- **The dream, instance #1:** Kain amalgamations as one-file GitHub repos.
  No build system, no tree, no lockfiles. The source *is* the distribution.
  Compile it anywhere Kain runs and you get the identical native binary.
- Compiled from the MarkScript blade of the Kain repository via
  `kain amalgamate src/ --raw` (live import closure: main, cli, types,
  lexer, parser, vm, bridge, import, error, jit, registry).
- Markdown constructs compile to bytecode (`OP_ENTER_DOMAIN`,
  `OP_ROUTINE_HEADER`, `OP_EXECUTE_CALL`, …) executed on a register VM with
  95 built-in handlers, JIT self-test, and an intent registry.

## Releases

Binaries (`mks-windows-x86_64.exe`, …) ship as GitHub Release assets —
built from exactly the `markscript.kn` in that tag, nothing else.
Reproduce any release: check out the tag, run the two commands above.

## License

MIT — see LICENSE.
