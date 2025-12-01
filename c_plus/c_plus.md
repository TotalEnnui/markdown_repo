# C++

## Standard template layout

C++ Standard Library

- Standard Template Library (STL)
  - Containers: vector, map, set, list, deque
  - Algorithms: sort, find, accumulate, etc.
  - Iterators: input, output, forward, bidirectional, random access
  - Function Objects: less, greater, custom functors
  - Smart Pointers: unique_ptr, shared_ptr, weak_ptr
- Concurrency: thread, mutex, future, atomic
- Filesystem: filesystem (C++17+)
- Time Utilities: chrono (duration, time_point, clocks)
- Regular Expressions: regex
- Type Traits & Metaprogramming: enable_if, is_same, declval, etc.
- Variant & Optional: variant, optional, any
- I/O Streams: cin, cout, cerr, file streams
- Localization: locale, codecvt
- Math & Numerics: complex, valarray, random, numeric_limits

---

## The Layers of the C++ Ecosystem

> Language standard (ISO C++) Defines the rules of the language itself — syntax, semantics, standard library.

| Component          | Examples                                  | Descriptions                                                                      |
|--------------------|-------------------------------------------|-----------------------------------------------------------------------------------|
| Compilers          | Clang, GCC, MSVC, Intel                   | Translate C++ source into machine code.                                           |
| Linkers            | GNU ld, MSVC link.exe, LLVM lld, Mold     | Stitch together object files and libraries into executables or shared libs.       |
| Build systems      | Make, Ninja, MSBuild, NMake               | Orchestrate compilation and linking, handle dependencies, parallelism.            |
| Meta-build systems | CMake, Meson, Bazel                       | Generate input for the build systems, abstracting away platform/compiler quirks.  |
| IDEs and editors   | CLion, Visual Studio, VS Code, Qt Creator | Wrap everything in a developer-friendly cockpit with debug, refactor, and tooling.|
