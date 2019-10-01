# Der Fehler

[![Documentation](https://docs.rs/fehler/badge.svg)](https://docs.rs/fehler/*/fehler/)

Der Fehler is a small but very opinionated Rust error handling library.

In many ways, der Fehler is a successor to failure. However, unlike failure,
der Fehler is built around the standard library's `Error` trait, which has
adopted the necessary improvements that the `Fail` trait had provided thanks to
[RFC 2504](https://github.com/rust-lang/rfcs/blob/master/text/2504-fix-error.md).

Der Fehler provocatively and unapologetically uses the terminology of
exceptions.

Der Fehler provides these items:

### The `Exception` type

Exception is a polymorphic error type, essentially a trait object. It is
similar to the `failure::Error` type, but for the `Error` trait in std. There
are a few key improvements:

* Like `failure::Error` is possible to construct an ad hoc error from any type
  that implements `Debug` and `Display` (such as a string). Unlike
  `failure::Error`, it can be properly downcast to that type.
* Unlike `failure::Error`, `Exception` is guaranteed to be the size of a single
  narrow pointer (`failure::Error` is the size of a wide pointer).

Otherwise, it is roughly the same type: an Error trait object that guarantees
the presence of a backtrace.

### The `throw!` macro

`throw!` is a macro which is equivalent to the `Err($e)?` pattern. It takes an
error type and "throws" it.

### The `#[throws]` attribute

The throws attribute modifies a function or method to make it return a
`Result`. It takes an optional typename as an argument to the attribute which
will be the error type of this function; if no typename is supplied, the error
type is `Exception`.

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

### The `error!` macro

This macro constructs an ad hoc error from format strings, similar to the
`format!` macro.

### The `Context` trait

This crate also defines a `Context` trait for the `Result` type, which
contains a `context` method for injecting context around an error.

# TODO

* Possibly add a Display derive
* Make throws work on closures and async blocks (attributes are not allowed on
  expressions on stable)

## License

Licensed under either of

 * Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

#### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
