[![version-badge][]][version] [![license-badge][]][license] [![rust-version-badge][]][rust-version]

Adding support for "throwing functions" to Rust through procedural macros.
Functions marked with the `throws` attribute return `Result`, but the "Ok" path
is used by default and you don't need to wrap ok return values in `Ok`. To throw
errors, use `?` or the `throws` macro.

> A fork of [Der Fehler](https://github.com/withoutboats/fehler) updating
dependencies and fixing issues while (by my understanding) boats is unable to
contribute to open source.

# The `#[throws]` attribute

The throws attribute modifies a function or method to make it return a
`Result`. It takes an optional typename as an argument to the attribute which
will be the error type of this function; if no typename is supplied, it uses
the default error type for this crate.

Within the function body, `return`s (including the implicit final return) are
automatically "Ok-wrapped." To raise errors, use `?` or the `throws!` macro.

For example, these two functions are equivalent:

```rust
#[throws(i32)]
fn foo(x: bool) -> i32 {
    if x {
        0
    } else {
        throw!(1);
    }
}

fn bar(x: bool) -> Result<i32, i32> {
    if x {
        Ok(0)
    } else {
        Err(1)
    }
}
```

## In functions that return `Option`

The attribute can be used to make a function that returns an Option using the
`as Option` syntax, demonstrated below:

```rust
// This function returns `Option<i32>`
#[throws(as Option)]
fn foo(x: bool) -> i32 {
    if x {
        0
    } else {
        throw!();
    }
}
```

# The `throw!` macro

`throw!` is a macro which is equivalent to the `Err($e)?` pattern. It takes an
error type and "throws" it.

One important aspect of the `throw!` macro is that it allows you to return
errors inside of functions marked with `throws`. You cannot just `return`
errors from these functions, you need to use this macro.

# Rust Version Policy

This crate only supports the current stable version of Rust, patch releases may
use new features at any time.

# License

Licensed under either of

 * Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.

[version-badge]: https://img.shields.io/crates/v/culpa.svg?style=flat-square
[version]: https://crates.io/crates/culpa
[license-badge]: https://img.shields.io/crates/l/culpa.svg?style=flat-square
[license]: #license
[rust-version-badge]: https://img.shields.io/badge/rust-latest%20stable-blueviolet.svg?style=flat-square
[rust-version]: #rust-version-policy

