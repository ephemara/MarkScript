# EXAMPLES_V2 — Torture Battery Report

**Exe under test:** `mks-windows-x86_64.exe` (release v1.0.0 asset, built from
`markscript.kn` in this repo). **Method:** `check` + `run` every file,
`jit-run` + `jit` where applicable. Asserts are silent on pass — every
`Handler error` line below is a real failure, every silence is a pass
(verified by planting known-false asserts: they DO scream).

## Results

| File | check | run | Verdict |
|---|---|---|---|
| 01_lexer_torture | PASS | exit 0, **1 dispatch of dozens** | ⚠️ Compiles everything (spec holds: zero syntax errors incl. h7, ragged tables, unicode, unclosed sections) but only one intent fires |
| 02_compute_gauntlet | PASS | exit 0, **fully silent** | ✅ fib/collatz/primes/triangle/100k-loop asserts ALL pass — VM integer math is CORRECT. Prints never fire (see F3) |
| 03_intent_storm | PASS | exit 0, **zero dispatches** | ⚠️ No intent fires, no fs side effects created. Same shape as 05 which fires 10 — dispatch scoping undetermined (see F3) |
| 04_matrix_madness | PASS | exit 0, **1 assert FAILS** | ❌ `matrix_tables != 7` — fence-set var invisible to later intent (see F2) |
| 05_error_alley | PASS | exit 0, graceful | ✅ Unknown intent, arity error, div-zero, assert-fail, import-fail ALL handled, no crash. Multi-word print args garbled (see F4) |
| 06_jit_brutal (VM) | PASS | **SEGFAULT 0xC0000005** | ❌❌ 2 assert fails (same F2 pattern) then access violation. Fuzzer-grade find |
| 06_jit_brutal (jit-run) | — | exit 0, **no execution evidence** | ❌ Compiles to x86-64, then silence. The asserts that fail under VM should fail here — nothing runs |
| 07_fuzz_corpus | PASS (+2 correct unknown-intent warnings) | exit 0, 4 dispatches, no hang | ✅ Unclosed fence, nested quotes, adversarial nesting all survive. Lexer/parser robust |
| `jit` selftest | — | all op emitters OK | ✅ JIT backend alive, emits bytes for every op |
| `repl` | — | canned demo, ignores stdin | ❌ Not scriptable, not interactive without TTY |
| `eval` | — | "Nothing to execute" (print, assert) | ❌ Stub |
| `pipe` | — | silent exit 0 (full doc in, nothing out) | ❌ Stub |

## Findings (all reproduced, all in this repo's exe)

**F1 — String constant table broken (`<invalid str_ref>`).** Any string
literal pushed via `OP_PUSH_STRING_REF` renders `<invalid`. Measured: 74 in
fizzbuzz, 10 in the repo's own `calculator_suite.md`. Poisons ALL string
output. Root cause lives in the VM's `str_consts` threading (indices emitted
by codegen don't match the table at exec). **This is the MarkScript bug —
not toolchain, not env** (proven: Kain-side integer/string lanes verified
independently).

**F2 — Fence→intent variable visibility intermittent.** Identical pattern
(fence sets var, `> assert` reads it): passes in 02 + probes, fails in 04
(`matrix_tables`) and 06 (`ja`/`jb`). Suspect: routine/table-count-dependent
scope or var-slot collision. Needs a one-variable-at-a-time repro.

**F3 — Top-level intent dispatch inconsistent.** Probe matrix: lone `> print`
never fires; fence+prints never fire; yet 05 fires 10 dispatches and 01 fires
1. `handler_println` prints `args[0]` ONLY (bridge.kn:722-725) — multi-word
strings can never print whole by construction.

**F4 — Print args garbled.** `> print "divzero row done"` → `[PRINT] divzero`;
other multi-word prints → `[PRINT] 0`. Bare words resolve as unset vars (→0);
quoting does not protect spaces. Intent tokenizer suspect.

**F5 — Segfault after assert failures** (06: exit -1073741819). Error path
corrupts the VM loop. Highest severity in this report.

**F6 — `jit-run` compiles, never executes.** No output, no assert failures,
exit 0 on input that fails loudly under `run`.

**F7 — `disasm` and `run` disagree**: 943 vs 414 ops on 02_compute_gauntlet.
Two different frontends; at least one's count is fiction.

**F8 — `repl`/`eval`/`pipe` are stubs.** Canned demo / nothing-to-execute /
silent-exit. Files (`run`) are the only working execution path.

## Portability note (good news)

No `../std/intents.md` beside the input → exe falls back to 95 embedded
handlers and runs anyway. V2 files execute from ANY directory. Registry file
is enhancement, not requirement.

## Repro

```bat
mks.exe check EXAMPLES_V2\04_matrix_madness.md   :: passes
mks.exe run   EXAMPLES_V2\04_matrix_madness.md   :: assert matrix_tables fails
mks.exe run   EXAMPLES_V2\06_jit_brutal.md       :: asserts fail, then SEGFAULT
mks.exe jit-run EXAMPLES_V2\06_jit_brutal.md     :: silent exit 0 (should fail same asserts)
echo ... | mks.exe repl                          :: canned demo, stdin ignored
```
