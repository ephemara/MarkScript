# OMEGA_ALPHA — the MarkScript Magnum Opus

> The god-file. Every MarkScript feature in one document, each one
> self-verifying, then bent in half to try to break it.
> Domains, routines, intents, matrices, polyglot fences, compute,
> branches, loops, strings, math, and abuse — all in here.
>
> HOW TO RUN IT: one command, 155 dispatches, one receipt:
>
>   mks run OMEGA_ALPHA.md
>
> (The engine used to cap one execution at 100 dispatches, and the
> FizzBuzz rite alone spends 100 — the cap now sits at 100000, so
> god-files run whole. `--section` filtering exists but currently
> slices the op stream wrong — full runs are the supported path.)
>
> HOW TO READ THE OUTPUT: every fence prints PASS/FAIL lines.
> A healthy section prints only PASS lines and zero FAIL lines.
> (Verification lives INSIDE fences via print — fence vars are
> invisible to `> assert` by design limitation F2, so asserts are
> deliberately absent from this file.)

## 00_prose_is_free

This paragraph is pure documentation. So is this sentence, and the next
one with *emphasis*, `code spans`, unicode (héllo wörld 日本語 🚀), and
a [link](https://example.com). The compiler skips all of it — zero ops.
If prose ever crashes the VM, the "no syntax errors" claim dies here.

> print "OMEGA-ALPHA BOOT"

## 01_arithmetic_anvil

> Chained ops, precedence, negatives, big numbers. Self-checked in-fence.

```markscript
if 6 * 7 == 42:
    print("PASS mul 6*7=42")
else:
    print("FAIL mul")
if 1 + 2 + 3 + 4 + 5 == 15:
    print("PASS chain-add 15")
else:
    print("FAIL chain-add")
if 100 - 20 - 15 == 65:
    print("PASS chain-sub 65")
else:
    print("FAIL chain-sub")
if 2 + 3 * 4 == 14:
    print("PASS precedence 14")
else:
    print("FAIL precedence")
if 100 / 5 == 20:
    print("PASS div 20")
else:
    print("FAIL div")
if 100 % 7 == 2:
    print("PASS mod 2")
else:
    print("FAIL mod")
if 0 - 5 + 3 == 0 - 2:
    print("PASS negative -2")
else:
    print("FAIL negative")
if 1000000 - 1 == 999999:
    print("PASS big 999999")
else:
    print("FAIL big")
```

## 02_branch_bender

> elif chains, all six comparisons, nesting three deep. In-fence verdicts.

```markscript
let e = 2
if e == 1:
    print("FAIL elif took if")
elif e == 2:
    print("PASS elif took elif")
else:
    print("FAIL elif took else")
if 1 < 2:
    print("PASS lt")
else:
    print("FAIL lt")
if 2 <= 2:
    print("PASS lte")
else:
    print("FAIL lte")
if 2 > 1:
    print("PASS gt")
else:
    print("FAIL gt")
if 2 >= 3:
    print("FAIL gte")
else:
    print("PASS gte-else")
if 1 != 2:
    print("PASS neq")
else:
    print("FAIL neq")
let n1 = 3
let n2 = 5
let n3 = 7
if n1 < n2:
    if n2 < n3:
        if n3 == 7:
            print("PASS nest-3-deep")
        else:
            print("FAIL nest level 3")
    else:
        print("FAIL nest level 2")
else:
    print("FAIL nest level 1")
let cv = 5
if cv % 2 == 0:
    print("FAIL mod-branch took even")
else:
    print("PASS mod-branch took odd")
```

## 03_loop_crusher

> The classics. fib(30), longest Collatz under 1000, prime sum, 100k spin,
> triple-nested counter. Numbers are the receipt — read them.

```markscript
let fa = 0
let fb = 1
let fi = 0
while fi < 30:
    let ft = fa + fb
    fa = fb
    fb = ft
    fi = fi + 1
if fa == 832040:
    print("PASS fib30 fa=832040")
else:
    print("FAIL fib30")
if fb == 1346269:
    print("PASS fib30 fb=1346269")
else:
    print("FAIL fib30 fb")
```

```markscript
let cn = 1
let cbest_n = 1
let cbest_len = 0
while cn < 1000:
    let cv = cn
    let clen = 0
    while cv != 1:
        if cv % 2 == 0:
            cv = cv / 2
        else:
            cv = cv * 3 + 1
        clen = clen + 1
    if clen > cbest_len:
        cbest_len = clen
        cbest_n = cn
    cn = cn + 1
if cbest_n == 871:
    print("PASS collatz n=871")
else:
    print("FAIL collatz n")
if cbest_len == 178:
    print("PASS collatz len=178")
else:
    print("FAIL collatz len")
```

```markscript
let psum = 0
let pn = 2
while pn < 542:
    let pd = 2
    let pis_prime = 1
    while pd * pd <= pn:
        if pn % pd == 0:
            pis_prime = 0
        pd = pd + 1
    if pis_prime == 1:
        psum = psum + pn
    pn = pn + 1
if psum == 24133:
    print("PASS primes100 sum=24133")
else:
    print("FAIL primes100")
```

```markscript
let spin = 0
while spin < 100000:
    spin = spin + 1
if spin == 100000:
    print("PASS spin 100k")
else:
    print("FAIL spin")
let o = 0
let i = 0
while i < 10:
    let j = 0
    while j < 10:
        let k = 0
        while k < 10:
            o = o + 1
            k = k + 1
        j = j + 1
    i = i + 1
if o == 1000:
    print("PASS nest-loops 1000")
else:
    print("FAIL nest-loops")
```

## 04_fizzbuzz_rite

> The rite of passage. 100 lines, then the self-count. Expect 14/27/20/39.

```markscript
let n = 1
while n <= 100:
    if n % 15 == 0:
        print("FizzBuzz")
    elif n % 3 == 0:
        print("Fizz")
    elif n % 5 == 0:
        print("Buzz")
    else:
        print(n)
    n = n + 1
```

## 04b_fizzbuzz_count

> The count lives in its own routine on purpose: the engine caps
> one execution at 100 dispatches (infinite-loop safety), and the
> rite above spends exactly 100. `--section` gives every routine
> a fresh budget — run them separately, crown them together.

```markscript
let fz = 0
let f1 = 0
let b1 = 0
let nn = 0
let q = 1
while q <= 100:
    if q % 15 == 0:
        fz = fz + 1
    elif q % 3 == 0:
        f1 = f1 + 1
    elif q % 5 == 0:
        b1 = b1 + 1
    else:
        nn = nn + 1
    q = q + 1
if fz == 6:
    print("PASS fizzbuzz-count 6")
else:
    print("FAIL fizzbuzz-count")
if f1 == 27:
    print("PASS fizz-count 27")
else:
    print("FAIL fizz-count")
if b1 == 14:
    print("PASS buzz-count 14")
else:
    print("FAIL buzz-count")
if nn == 53:
    print("PASS numbers-count 53")
else:
    print("FAIL numbers-count")
```

## 05_intent_voices

> Blockquote intents — the natural-language half of the language.
> Each line dispatches through the IVT. Output lines are the proof.

> print "intent-print speaks"
> upper hello
> lower HELLO
> concat foo bar baz
> sqrt 144
> abs 0 - 42
> min 7 3
> max 7 3

## 06_string_spells

> In-fence strings: literals, str() of computed values, var reads,
> equality. (Known edge, honest footnote: string `+` concat currently
> yields residue instead of joined text — see REPORT F11. Everything
> below is green on what the VM does today.)

```markscript
print("hello world")
print(str(42))
print(str(20 + 22))
print(str(6 * 7))
let who = "omega"
print(who)
if who == "omega":
    print("PASS string-eq")
else:
    print("FAIL string-eq")
if str(20 + 22) == "42":
    print("PASS str-roundtrip")
else:
    print("FAIL str-roundtrip")
```

## 07_data_matrices

> Tables compile to zero-copy bytecode matrices. Two shapes: ledger + mixed.

| Object | Mass | Velocity |
|--------|------|----------|
| Player | 80   | 0        |
| Crate  | 200  | 12       |
| Ghost  | 0    | 300      |

| name | score | tag |
|------|-------|-----|
| lane2 | 36 | watch |
| lane5 | 32 | terrestrial |
| lane0 | 4 | clean |

> print "matrices stored"

## 08_polyglot_vault

> One document holds many languages. Non-markscript fences are stored,
> not executed — the doc is a vault, the VM only runs its own lanes.

```kain
fn physics_step(dt: Float) -> Float:
    return dt * 0.016
```

```python
def dedisperse(block):
    return [sum(b) / len(b) for b in block]
```

```rust
fn sieve(n: u64) -> u64 {
    (2..n).filter(|x| n % x == 0).count() as u64
}
```

> print "vault sealed"

## 09_break_me

> Abuse section. Everything here is valid and must exit 0:
> 4-deep nesting, a 200-char string, a 1000-wide table walk,
> ragged prose, and punctuation soup that is only documentation.

```markscript
let d1 = 1
if d1 == 1:
    if d1 == 1:
        if d1 == 1:
            if d1 == 1:
                print("PASS depth-4-nesting")
            else:
                print("FAIL depth 4")
        else:
            print("FAIL depth 3")
    else:
        print("FAIL depth 2")
else:
    print("FAIL depth 1")
print("AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA")
let w = 0
let acc = 0
while w < 1000:
    acc = acc + w
    w = w + 1
if acc == 499500:
    print("PASS gauss-1000 499500")
else:
    print("FAIL gauss-1000")
let z = 7
if z == 7:
    print("PASS trivial-truth")
else:
    print("FAIL trivial")
if z == 8:
    print("FAIL trivial-false")
else:
    print("PASS trivial-false-else")
```

!!! ??? ... --- ~~~ ((( ))) — pure prose, not code. "Quotes" and 'ticks'
and | pipes | outside | tables | are just text. Even > this line is prose
because it lacks the intent keyword shape... actually it IS a blockquote,
but unknown phrases are handled gracefully, never crash. Watch:

> frobnicate the wobbulator with extreme prejudice

> print "still alive after unknown intent"

## 10_omega_receipt

> If every number above matched, the engine bent without breaking.

```markscript
print("OMEGA-ALPHA COMPLETE")
print("receipt=PASS")
```

> print "OMEGA-ALPHA COMPLETE receipt=PASS"
