# Snek VM — Mark & Sweep Garbage Collector

## Overview

Snek VM is a lightweight virtual machine written in C that demonstrates a **mark-and-sweep garbage collection (GC)** system for dynamically allocated objects. It replaces naive reference counting to correctly handle **cyclic references** and prevent memory leaks.

---

## Key Features

* 🧠 **Mark-and-Sweep GC**

  * Detects and frees unreachable objects
  * Handles cyclic references (unlike reference counting)

* 🧱 **Custom Object System**

  * Supports: `INTEGER`, `FLOAT`, `STRING`, `VECTOR3`, `ARRAY`
  * Tagged union-based design for polymorphism

* 📦 **Stack-Based VM**

  * Frames simulate scope
  * Objects tracked globally for GC
  * Generic dynamic stack reused across modules

* 🔍 **Explicit Memory Management**

  * Manual allocation (`malloc`) and cleanup
  * GC integrated into VM lifecycle

---

## Architecture

```
          +------------------+
          |      main.c      |
          +--------+---------+
                   |
                   v
          +------------------+
          |       VM         |
          |  (vm.c / vm.h)   |
          +--------+---------+
           | Frames | Objects|
           v        v
     +--------+   +--------+
     | frame  |   | snek   |
     | stack  |   | objects|
     +--------+   +--------+
           \        /
            \      /
         +----------------+
         |   stack.c      |
         | Generic Stack  |
         +----------------+
```

---

## Garbage Collection Algorithm

### 1. Mark Phase

* Mark objects referenced by **active stack frames**

### 2. Trace Phase

* Traverse object graph (arrays, vectors)
* Mark all **reachable objects**

### 3. Sweep Phase

* Free all **unmarked objects**
* Reset marks for next cycle

---

## Example Problem (Why GC?)

Reference counting fails with cycles:

```c
first -> second
second -> first
```

Both objects remain in memory forever ❌

Mark-and-sweep solves this by checking **reachability**, not reference count.

---

## Project Structure

```
.
├── main.c          # Test harness
├── vm.c / vm.h     # VM + GC implementation
├── sneknew.c/h     # Object creation
├── snekobject.c/h  # Object operations & freeing
├── stack.c/h       # Generic stack
```

---

## Build & Run

```bash
gcc *.c -o snek_vm
./snek_vm
```

---

## Limitations

* ❌ No instruction set / bytecode execution
* ❌ Single-threaded (not concurrency-safe)
* ❌ Minimal error handling
* ❌ Limited test coverage

---

## Future Improvements

* Add bytecode interpreter
* Introduce logging & robust error handling
* Improve GC (generational / incremental)
* Add unit tests + CI
* Memory profiling (e.g., Valgrind)

---

## Takeaways

* Demonstrates **core GC concepts** in a minimal VM
* Shows why **reference counting fails with cycles**
* Provides a foundation for building a real interpreter/runtime
