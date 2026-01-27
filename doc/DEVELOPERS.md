# Developer Guide

This document describes the development workflow for **pgmoneta_mcp**.

It is intended for developers who want to build, test, debug, or extend the project.

- For contribution rules and PR workflow, see [CONTRIBUTING.md](../CONTRIBUTING.md)
- For user setup and runtime configuration, see [GETTING_STARTED.md](GETTING_STARTED.md)

---

## Prerequisites

- Rust toolchain (stable), preferably installed via [rustup](https://rustup.rs)
- `cargo` (included with Rust)
- `git`
- A running **pgmoneta** instance for integration testing

On Linux, some distributions provide useful system packages:

```bash
# Fedora / RHEL
sudo dnf install git rustfmt clippy

# Debian / Ubuntu
sudo apt-get install git cargo rustfmt clippy
```

Using **rustup** is recommended for consistent toolchain management across platforms.

---

## Building

All build tasks are handled by Cargo.

### Debug build

```bash
cargo build
```

### Release build

```bash
cargo build --release
```

Binaries are placed in:

* `target/debug/`
* `target/release/`

---

## Formatting and Linting

Code formatting and linting are enforced by CI.

### Format code

```bash
cargo fmt --all
```

### Check formatting (CI mode)

```bash
cargo fmt --all --check
```

### Run Clippy

```bash
cargo clippy
```

### Clippy with warnings as errors (CI)

```bash
cargo clippy -- -D warnings
```

All Clippy warnings must be resolved before submitting a pull request.

---

## Testing

### Run all tests

```bash
cargo test
```

### Run tests with output

```bash
cargo test -- --nocapture
```

### Run tests matching a pattern

```bash
cargo test <pattern>
```

---

## Running and Debugging

### Run server during development

```bash
cargo run --bin pgmoneta-mcp-server -- -c pgmoneta-mcp.conf -u pgmoneta-mcp-users.conf
```

### Run built binaries directly

```bash
./target/debug/pgmoneta-mcp-server -c <config> -u <users>
./target/debug/pgmoneta-mcp-admin --help
```

### Debugging

Rust debugging can be done using:

```bash
rust-lldb target/debug/pgmoneta-mcp-server
# or
rust-gdb target/debug/pgmoneta-mcp-server
```

VS Code users can debug using the **CodeLLDB** extension.

---

## Logging

The project uses the `tracing` ecosystem for logging.

Logging is primarily configured via the configuration file.  

---

## Continuous Integration

The project uses GitHub Actions for CI. The pipeline includes:

- **Formatting**: Ensures code adheres to style guidelines using `cargo fmt`.
- **Linting**: Checks for common issues using `clippy`.
- **Build Validation**: Verifies the code compiles using `cargo check`.

To run these checks locally:

```bash
cargo fmt --all --check
cargo clippy --all-targets --all-features
cargo check
```

---

## License Headers

This project uses [`licensesnip`](https://github.com/notken12/licensesnip) to ensure source files include correct license headers.

If you haven't already installed licensesnip, you can do so using Cargo:

```bash
cargo install licensesnip
```

When adding new source files, run `licensesnip` from the project root:

```bash
licensesnip
```

The license template is defined in `.licensesnip`. Running `licensesnip` will automatically insert the correct header where needed.

---

## Contributing Notes

* Add yourself to the `AUTHORS` file in your first pull request
* When committing, use the format `[#issue_number] commit message`.
* Keep commits small, focused, squashed, and rebased before merging
* Follow the workflow described in [CONTRIBUTING.md](../CONTRIBUTING.md)

---

Thank you for contributing to **pgmoneta_mcp**!