![Cyan's logo](./Logo.png)

Cyan is a modern C++26 project built around three core principles: **safety, performance, and ergonomics**.

Leveraging the latest language features and novel safety mechanisms, Cyan pushes the boundaries of high-performance systems programming, delivering a robust and expressive programming experience without compromising execution speed, correctness, or developer control.

---

## Design Philosophy

### Safety

Cyan is designed with safety as a first-class concern. Memory safety, type safety, and well-defined behavior are enforced wherever possible through language-level guarantees and careful API design. Undefined behavior is considered a defect, never an acceptable engineering trade-off.


### Performance

No trade-off. No compromise. Just performance, engineered to be safe.

Cyan demonstrates that high-performance systems programming does not need to sacrifice memory safety, correctness, or developer control. You shouldn't have to choose between writing fast software and writing safe software. With Cyan, you can demand both.


### Ergonomics

Good software should be a pleasure to use. Cyan prioritizes clean, expressive APIs, clear error messages, and a smooth developer experience from first build to production deployment.

---

## Requirements

### Compilers

Cyan requires a compiler with C++26 support. The following compilers are recommended:

| Compiler | Minimum Version |
|----------|-----------------|
| Clang    | 23+             |
| GCC      | 16+             |
| MSVC     | 19.52+          |


### Build System

| Tool  | Minimum Version |
|-------|-----------------|
| CMake | 3.30+           |

---

## Getting Started

### WARNING: GCC `-Wshadow` Compatibility

When `core/safety.hpp` is included, Cyan unconditionally disables GCC's `-Wshadow` diagnostic because some of its fundamental features intentionally rely on name shadowing.

This diagnostic suppression is propagated to consuming projects to ensure compatibility with Cyan's public interfaces.

We apologize for the inconvenience.

We believe GCC could benefit from more granular diagnostic controls. MSVC and Clang already allow that. We encourage the community to propose an enhancement to the GCC compiler team.


### Clone

```bash
git clone https://github.com/salonsoro/cyan.git
cd cyan
```


### Build

To integrate Cyan into your project, do the following:

```cmake
target_link_libraries(your_target PRIVATE Cyan::Cyan)
```

Then, to build with the tests enabled, invoke CMake in the following way:

```bash
cmake -B Build/{Debug|Release} -S . -G Ninja -D BUILD_TESTING=On -D CMAKE_BUILD_TYPE={Debug|Release}
cmake --build Build/{Debug|Release} --target CYAN_RUN_TESTS
```

---

## Documentation

The `doc` folder contains a vanilla JavaScript & HTML5/CSS3 web platform with extensive documentation on all the features.

If something is not documented, it's not meant to be used.

---

## Testing

Every documented feature is thoroughly tested under the test folder and validated through the CI pipeline.

The tests also showcase how the features are meant to be used.

---

## License

See [LICENSE](LICENSE.md) for details.