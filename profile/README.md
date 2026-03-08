# NextStd

![logo](./assets/logo.png)

> Working toward a safer standard library for the next generation

**NextStd** is an experimental effort to explore safer, well-defined, and
modern alternatives to traditional C standard library components. The project
focuses on correctness, explicit behavior, and performance, while remaining
practical for real-world systems and embedded use.

This is a work in progress. APIs, guarantees, and scope may evolve as the
project matures. All core logic is backed by Rust, safely exposed to C via FFI.

---

## Implemented Components

### 1. `ns_io` (Type-Safe I/O)

A memory-safe, crash-proof alternative to `<stdio.h>` that completely eliminates
format string vulnerabilities. It uses C11 `_Generic` routing to automatically
detect data types at compile time.

* **Output (`ns_print`, `ns_println`):** Safely prints `int`, `float`, `double`
, `char*`, and custom NextStd types without `%d` or `%s` specifiers.
* **Input (`ns_read`):** Safely reads `int`, `float`, and `double`. It
gracefully defaults to `0` on invalid user input instead of panicking,
and includes strict `NULL` pointer safeguards.

### 2. `ns_string` (Memory-Safe Strings)

A modern alternative to standard C strings (`char*`) designed to eliminate
buffer overflows, $O(N)$ length calculations, and double-free vulnerabilities.

* **Small String Optimization (SSO):** Strings under 24 bytes are allocated
directly on the stack, resulting in zero heap fragmentation
(ideal for embedded constraints).
* **Automatic Heap Scaling:** Strings 24 bytes and larger seamlessly
scale to the heap, with memory lifecycles safely managed by the Rust backend.
* **Core Functions:** `ns_string_new()`, `ns_string_free()`

---

## Architecture

NextStd bridges the gap between modern safety and legacy compatibility:

1. **Rust Backend:** Handles memory allocation, bounds checking, and core logic.
2. **C Frontend:** Exposes a clean, macro-driven API that feels completely
native to C developers.
