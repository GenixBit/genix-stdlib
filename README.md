# Genix Standard Library

The official standard library for the **Genix programming language** by **GenixBit**.

> Status: early development / pre-alpha. APIs are expected to change.

## Purpose

`genix-stdlib` provides the batteries-included APIs that most `.gb` programs should be able to rely on without third-party dependencies.

## Planned modules

- `core` — fundamental language-facing types and utilities
- `collections` — lists, maps, sets, iterators
- `io` — input/output helpers
- `fs` — files and directories
- `path` — portable path handling
- `json` — JSON encoding and decoding
- `math` — numeric utilities
- `time` — dates, durations, clocks
- `net` — networking primitives
- `http` — HTTP client/server foundations
- `crypto` — safe cryptographic interfaces
- `process` — environment and process control
- `concurrent` — concurrency utilities
- `test` — testing primitives

## Design goals

- Consistent APIs
- Strong typing
- Safe defaults
- Cross-platform behavior where practical
- Minimal hidden magic
- Clear separation between stable core APIs and experimental packages

## Example direction

```gb
use fs

fn main() {
    let text = fs.read_text("hello.txt")?
    print(text)
}
```

The syntax above represents the intended direction and is not yet a stable language contract.

---

**Genix — by GenixBit**
