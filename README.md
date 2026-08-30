# Genix Standard Library

The official standard library for the **Genix programming language** by **GenixBit**.

> Status: **pre-alpha / 0.0.1**. APIs may change before the first stable Genix release.

## Compatibility

```text
GENIX_STDLIB_VERSION=0.0.1
GENIX_LANGUAGE_VERSION=0.0.1
GENIX_RUNTIME_ABI=1
```

The compiler validates `COMPATIBILITY` before loading official stdlib modules.

## Setup

```bash
export GENIX_STDLIB=/path/to/genix-stdlib
export GENIX_RUNTIME=/path/to/genix-runtime
```

Project-local modules take precedence over standard-library modules with the same name.

## Implemented modules

### `io`

```text
io.println(text: string)
io.print_int(value: int)
io.print_float(value: float)
io.print_bool(value: bool)
io.input(prompt: string) -> string
```

### `fs`

Preferred recoverable APIs:

```text
fs.try_read_text(path: string) -> Result<string,string>
fs.try_write_text(path: string, text: string) -> Result<bool,string>
```

Example:

```gb
import fs;

fn load(path: string) -> Result<string,string> {
    let text: string = fs.try_read_text(path)?;
    return Ok(text);
}
```

The original bootstrap APIs remain temporarily available for compatibility:

```text
fs.read_text(path: string) -> string
fs.write_text(path: string, text: string)
```

Those legacy forms may panic/terminate on host I/O failure in native mode, so new code should prefer the `try_` variants.

### `process`

Preferred optional environment lookup:

```text
process.env_option(name: string) -> Option<string>
```

Example:

```gb
import process;

fn main() {
    let home: Option<string> = process.env_option("HOME");

    match home {
        Some(value) => {
            print(value);
        }
        None => {
            print("HOME is not set");
        }
    }
}
```

Compatibility APIs:

```text
process.env(name: string) -> string
process.exit(code: int)
```

`process.env` still represents a missing variable as an empty string. New code should prefer `process.env_option` when absence matters.

### `math`

```text
math.abs_int(value: int) -> int
math.abs_float(value: float) -> float
math.min_int(a: int, b: int) -> int
math.max_int(a: int, b: int) -> int
math.clamp_int(value: int, minimum: int, maximum: int) -> int
math.square(value: float) -> float
```

### `string`

```text
string.concat(left: string, right: string) -> string
string.equals(left: string, right: string) -> bool
string.not_equals(left: string, right: string) -> bool
```

## Typed error handling

The current compiler supports primitive-payload forms of:

```text
Option<int|float|bool|string>
Result<int|float|bool|string, string>
```

with `Some`, `None`, `Ok`, `Err`, exhaustive `match`, and Result propagation with `?`.

Arbitrary user-defined generic payloads and custom error types are planned for later language revisions.

## Host intrinsic boundary

Most standard-library code remains ordinary `.gb` source. OS-facing operations are recognized by the compiler at a small canonical boundary.

Safe mappings include:

```text
process.env_option → host environment / gb_env_get_option
fs.try_read_text   → host filesystem / gb_fs_try_read_text
fs.try_write_text  → host filesystem / gb_fs_try_write_text
```

`gb run` implements equivalent host behavior through the Rust interpreter. `gb build` lowers these calls to the Genix Runtime ABI.

This remains the bootstrap intrinsic layer; a formal native/FFI declaration system is still planned.

## Repository layout

```text
genix-stdlib/
├── COMPATIBILITY
├── modules/
│   ├── io.gb
│   ├── fs.gb
│   ├── process.gb
│   ├── math.gb
│   └── string.gb
├── examples/
│   ├── smoke/
│   ├── host/
│   └── safe/
└── .github/
    └── workflows/
        └── ci.yml
```

## Architecture

```text
Genix application
      ↓
Genix stdlib
      ↓
portable .gb code + host intrinsic boundary
      ↓
Genix Runtime ABI
      ↓
Operating system
```

## Planned modules

- `core`
- `collections`
- `path`
- `json`
- `time`
- `net`
- `http`
- `crypto`
- `concurrent`
- `test`

## Validation

CI validates the stdlib against `genix-lang` and `genix-runtime`, including static type checking, Option/Result semantics, interpreter execution, typed IR generation, native compilation, safe host I/O, and native execution.

---

**Genix — by GenixBit**
