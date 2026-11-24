# Cyan — Agent Instructions

## 1. Project Overview

Cyan is a modern C++26 systems programming project built around three core principles:

- Safety
- Performance
- Ergonomics

Cyan provides a collection of abstractions, safety mechanisms, utilities, and programming facilities designed to enable high-performance systems programming without sacrificing correctness or developer control.

The project is independently developed and maintained. Its architecture and technical direction are controlled by its maintainer.

## 2. Core Technical Principles

The following constraints are fundamental to Cyan's design:

- **Language:** C++26.
- **Standard Library:** Minimal dependency on the C++ Standard Library.
- **Third-party dependencies:** Minimal.
- **Distribution:** A source-included CMake interface library.

Do not introduce dependencies on facilities that violate these constraints.

Do not weaken compiler requirements, safety guarantees, or architectural constraints merely to accommodate an implementation.

## 3. Development Environment

Cyan uses:

- CMake as its build-system generator.
- CTest for test execution.
- GCC, Clang, and MSVC as strongly recommended compiler families.

Consult the project's CMake configuration and CI workflows for the authoritative compiler versions, flags, platform requirements, and build configurations.

Never assume that a compiler extension is portable across all supported toolchains.

## 4. Repository Structure

The repository contains the following primary areas:

- `source/`: Cyan's implementation and public interfaces.
- `test/`: Automated tests and verification of Cyan's functionality.
- `doc/`: Cyan's documented features.

Additional directories should be inspected directly before making assumptions about their purpose.

When introducing new functionality, follow the existing organizational conventions.

## 5. Implementation Guidelines

Before implementing or modifying functionality:

1. Inspect the relevant headers, implementation files, and tests.
2. Understand the existing architecture and design constraints.
3. Check whether an equivalent facility already exists.
4. Verify compatibility with all supported compilers.
5. Add or update tests whenever functionality changes.

Additional requirements:

- Prefer compile-time guarantees where appropriate.
- Avoid unnecessary runtime overhead.
- Preserve API consistency.
- Avoid introducing undefined behavior.
- Do not silently weaken existing safety guarantees.
- Do not introduce unnecessary abstractions or boilerplate.
- Do not modify unrelated functionality.

Treat existing public interfaces and documented behavior as intentional unless there is evidence otherwise.

## 6. Build and Verification

Inspect the existing CMake configuration and CI workflows to determine the appropriate build and test commands.

After making changes:

- Build the affected targets.
- Run relevant tests.
- Check compiler diagnostics.
- Verify compatibility with applicable compiler configurations.

Never claim that a change has been tested unless the corresponding verification was actually performed.

If verification cannot be performed, explicitly report the limitation.

## 7. Development Boundaries

Cyan is not accepting external code contributions.

Coding agents may assist with internal development tasks when explicitly instructed by the maintainer.

Do not independently:

- Introduce new dependencies.
- Change fundamental architectural decisions.
- Change licensing terms.
- Remove safety mechanisms.
- Modify public API contracts without justification.
- Reformat or rewrite unrelated files.

When a requested change conflicts with a fundamental project constraint, explain the conflict before proceeding.

## 8. Documentation

Documentation must accurately reflect the implementation.

Do not document hypothetical functionality as implemented.

When modifying a public interface, review the corresponding documentation and update it when necessary.

Prefer concise, technically precise explanations over unnecessary verbosity.