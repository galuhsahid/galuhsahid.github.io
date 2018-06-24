---
layout: post
title: "rust-toolchain"
---

The `rust-toolchain` file lets you know which version of rustc is being used when building. To use it you just need to make a file called `rust-toolchain`, which content is the name of a single `rustup` toolchain. It can only contain: 

- Three release channels: stable, beta, nightly
- Rust version numbers (e.g. 1.0.0)
- Archive date (e.g. nightly-2017-01-01')

Say that your project is being tested against 1.0.0, this is what you would put in your `rust-toolchain` file:

```
1.0.0
```

And that's it.

The next question is, can `rust-toolchain` be overriden? The answer is yes. We must pay attention to the override precedence, from the highest to lowest precedence:

- An explicit toolchain, e.g. `cargo +beta`
- The `RUSTUP_TOOLCHAIN` env variable
- Directory override, e.g. `rustup override set beta`
- The `rust-toolchain` file
- The default toolchain

It should be noted that between the directory override and `rust-toolchain`, they are also preferred by their proximity to the current directory. A `rust-toolchain` file that is closer to the current directory will be preferred from the directory override that happens in a directory further away.

We can always make sure which toolchain is active by using `rustup show`.

[Source](https://github.com/rust-lang-nursery/rustup.rs#the-toolchain-file)