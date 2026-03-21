+++
title = "A Survey of Rust Strings"
date = 2024-09-26
description = "Examining the various string-related types in Rust."
+++

Why does Rust have so many string types? How are they
different, when should we use each one, and do they
have analogues in other programming languages?

<!-- more -->

This post will try to address these questions by traversing
the following families of types, roughly in order from least
abstract to most abstract.

- [Native Rust](#native)
    - [char](#char)
    - [u8, \[u8\], &\[u8\], and Vec\<u8\>](#u8)
    - [str, &str, and String](#str)
    - [&'static str](#static)
- [Foreign Function Interface (FFI)](#ffi)
    - [CStr and CString](#c)
    - [OsStr and OsString](#os)
    - [Path and PathBuf](#path)
- [Optimization](#optimization)
    - [Cow\<str\>](#cow)
    - [Small String Optimization (SSO)](#sso)

# Native Rust {#native}

These types are native in the sense that almost all Rust
code that interacts solely with other Rust code (vs.
with other programming languages or the operating system)
will stick to these types.

## char {#char}

What is a character, and how do we represent it in
a computer? Both questions are (perhaps surprisingly)
quite nuanced, and carry a lot of history.

As an American English-speaking programmer, I used to
think of a character as a 7-bit [ASCII](https://en.wikipedia.org/wiki/ASCII)-encoded value,
which C stores in its 8-bit [`char`](https://en.wikipedia.org/wiki/C_data_types#Main_types) type.



Today, while the world has mostly settled on [Unicode](https://home.unicode.org/),
there is still

Unlike C, where a char is ([at least](https://en.wikipedia.org/wiki/C_data_types#Main_types), but typically exactly) 1 byte

- [rustdoc](https://doc.rust-lang.org/std/primitive.char.html)

## u8, \[u8\], &\[u8\], and Vec\<u8\> {#u8}
    - [\[u8\]](https://doc.rust-lang.org/std/primitive.slice.html), &\[u8\], and [Vec\<u8\>](https://doc.rust-lang.org/std/vec/struct.Vec.html)

## str, &str, and String {#str}
    - [str](https://doc.rust-lang.org/std/primitive.str.html), &str, and [String](https://doc.rust-lang.org/std/string/struct.String.html)

## &'static str {#static}
    - &'static str

# Foreign Function Interface (FFI) {#ffi}

## CStr and CString {#c}
    - [std::ffi::CString](https://doc.rust-lang.org/std/ffi/struct.CString.html) and [std::ffi::CStr](https://doc.rust-lang.org/std/ffi/struct.CStr.html)

## Path and PathBuf {#path}
    - [std::path::PathBuf](https://doc.rust-lang.org/std/path/struct.PathBuf.html) and [std::path::Path](https://doc.rust-lang.org/std/path/struct.Path.html)

## OsStr and OsString {#os}
    - [std::ffi::OsString](https://doc.rust-lang.org/std/ffi/struct.OsString.html) and [std::ffi::OsStr](https://doc.rust-lang.org/std/ffi/struct.OsStr.html)

# Optimization {#optimization}

## Cow\<str\> {#cow}
    - [Cow\<str\>](https://doc.rust-lang.org/std/borrow/enum.Cow.html)

## Small String Optimization (SSO) {#sso}
    - [Small String Optimization (SSO)](https://github.com/rosetta-rs/string-rosetta-rs)
