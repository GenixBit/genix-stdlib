# Genix Standard Library

The official standard library for the **Genix programming language** by **GenixBit**.

> Status: **pre-alpha / 0.0.1**. APIs may change before the first stable Genix release.

## Compatibility

The standard library publishes machine-readable compatibility metadata in `COMPATIBILITY`.

Current compatibility:

```text
GENIX_STDLIB_VERSION=0.0.1
GENIX_LANGUAGE_VERSION=0.0.1
GENIX_RUNTIME_ABI=1
```

The Genix compiler validates this metadata before loading an official stdlib module.

## Use the standard library

During source development, point the compiler to this repository or an installed stdlib directory:

```bash
export GENIX_STDLIB=/path/to/genix-stdlib
```

Then a normal Genix project can import standard modules:

```gb
import io;
import math;
import string;

fn main() {
    io.println("Hello from Genix stdlib");

    let value: int = math.abs_int(-42);
    io.print_int(value);

    let message: string = string.concat("Hello ", "Genix");
    io.println(message);
}
```

Project-local modules take precedence over standard-library modules with the same name.

## Implemented modules

### `io`

Current API:

```text
io.println(text: string)
io.print_int(value: int)
io.print_float(value: float)
io.print_bool(value: bool)
```

These functions are written in Genix and currently delegate to the language `print(...)` primitive. Input APIs will be added after the native intrinsic/FFI boundary is formalized.

### `math`

Current API:

```text
math.abs_int(value: int) -> int
math.abs_float(value: float) -> float
math.min_int(a: int, b: int) -> int
math.max_int(a: int, b: int) -> int
math.clamp_int(value: int, minimum: int, maximum: int) -> int
math.square(value: float) -> float
```

### `string`

Current API:

```text
string.concat(left: string, right: string) -> string
string.equals(left: string, right: string) -> bool
string.not_equals(left: string, right: string) -> bool
```

## Repository layout

```text
genix-stdlib/
├── COMPATIBILITY
├── modules/
│   ├── io.gb
│   ├── math.gb
│   └── string.gb
├── examples/
│   └── smoke/
└── .github/
    └── workflows/
        └── ci.yml
```

## Architecture

Standard-library modules are ordinary `.gb` source modules. This keeps most high-level APIs portable and testable by both the interpreter and native compiler.

```text
Genix application
      ↓
Genix stdlib modules
      ↓
Language primitives / future intrinsics
      ↓
Genix Runtime ABI
      ↓
Operating system
```

Low-level functionality that requires operating-system access will use a documented compiler intrinsic / runtime ABI boundary rather than embedding platform-specific behavior directly in `.gb` modules.

## Planned modules

- `core`
- `collections`
- `fs`
- `path`
- `json`
- `time`
- `net`
- `http`
- `crypto`
- `process`
- `concurrent`
- `test`

`process` and other OS-facing APIs are intentionally not faked in the first release. They will be implemented after the native intrinsic/FFI contract is defined.

## Validation

CI validates the stdlib against the current `genix-lang` compiler and `genix-runtime`, including:

- Static type checking
- Interpreter execution
- Typed Genix IR generation
- Native compilation
- Execution of the generated native binary

---

**Genix — by GenixBit**
