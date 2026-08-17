# Setup osxcross

Github Action for setting up osxcross in a github action.

## Sources & Shoutouts

- [Thank you to James Waples for posting this article!](https://wapl.es/rust/2019/02/17/rust-cross-compile-linux-to-macos.html)

## Usage

```yaml
# Checkout your code
- name: Clone your Code
  uses: actions/checkout@v7

# Install the apple-darwin rust targets you need.
- name: Install Rust targets
  run: rustup target add x86_64-apple-darwin aarch64-apple-darwin

# Use this action
- uses: mbround18/setup-osxcross@v3
  # This builds executables & sets env variables for rust to consume.
  with:
    osx-version: "12.3"

# Build your code for apple-darwin based release
- name: Build Your Code (Intel)
  run: cargo build --release --target x86_64-apple-darwin

- name: Build Your Code (Apple Silicon)
  run: cargo build --release --target aarch64-apple-darwin
```

`osx-version` must point at an SDK that includes arm64 slices (macOS 11.0+, e.g. `12.3`) for the `aarch64-apple-darwin` target to be configured. Older SDKs will only get `x86_64-apple-darwin`, and the action will log a notice rather than failing.

## ZLIB and C/++ compilations

If you run into issues were you have zlib or have c as a dependenacy consider setting the following in your env.


```sh
# Make libz-sys (git2-rs -> libgit2-sys -> libz-sys) build as a statically linked lib
# This prevents the host zlib from being linked
export LIBZ_SYS_STATIC=1

# Use Clang for C/C++ builds
export CC=o64-clang
export CXX=o64-clang++
```

`AR_x86_64_apple_darwin` / `AR_aarch64_apple_darwin` are set automatically for you, so `cc`/`cc-rs` based builds pick up the right `ar` without extra configuration.


