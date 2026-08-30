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

Example:

```gb
import io;

fn main() {
    let name: string = io.input("Your name: ");
    io.println("Hello " + name);
}
```

### `fs`

```text
fs.read_text(path: string) -> string
fs.write_text(path: string, text: string)
```

Example:

```gb
import fs;

fn main() {
    fs.write_text("hello.txt", "Hello Genix");
    let text: string = fs.read_text("hello.txt");
    print(text);
}
```

### `process`

```text
process.env(name: string) -> string
process.exit(code: int)
```

A missing environment variable currently returns an empty string. Rich optional/result types will replace this simplified behavior once those language features exist.

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

## Host intrinsic boundary

Most standard-library code remains ordinary `.gb` source. OS-facing operations are recognized by the compiler at a small canonical boundary:

```text
io.input      → host stdin / gb_input
fs.read_text  → host filesystem / gb_fs_read_text
fs.write_text → host filesystem / gb_fs_write_text
process.env   → host environment / gb_env_get
process.exit  → host process / gb_process_exit
```

Users call the normal stdlib APIs; the compiler-specific mapping is an implementation detail. `gb run` provides equivalent behavior through the Rust interpreter, while `gb build` lowers the calls to Genix Runtime ABI services.

This is the bootstrap intrinsic layer. A formal native/FFI declaration system is still planned for external libraries and advanced platform integrations.

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
│   └── smoke/
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

CI validates the stdlib against `genix-lang` and `genix-runtime`, including static type checking, interpreter execution, typed IR generation, native compilation, and native execution.

---

**Genix — by GenixBit**
