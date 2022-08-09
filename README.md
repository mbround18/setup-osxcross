# Setup osxcross

Github Action for setting up osxcross in a github action.

## Sources & Shoutouts

- [Thank you to James Waples for posting this article!](https://wapl.es/rust/2019/02/17/rust-cross-compile-linux-to-macos.html)

## Usage

```yaml
# Setup Rust, use the x86_64-apple-darwin target but the rest can be customized.
- uses: ATiltedTree/setup-rust@v1
  with:
    targets: x86_64-apple-darwin
    rust-version: nightly

# Use the v1 of this action
- uses: mbround18/setup-osxcross@v1
  # This builds executables & sets env variables for rust to consume.
  with:
    osx-version: "12.3"

# Checkout your code
- name: Clone your Code
  uses: actions/checkout@v3

# Build your code for apple-darwin based release
- name: Build Your Code
  run: cargo build --release --target x86_64-apple-darwin
```

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


