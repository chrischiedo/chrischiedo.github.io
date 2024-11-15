---
layout: post
title: Getting started with Rust development
comments: true
tags:
  - performance
  - concurrency
  - Rust
  - borrow-checker
  - cargo
  - rustup
excerpt_separator: <!--more-->
---

Rust is a systems programming language that guarantees memory and thread safety through a rich type system and a unique ownership model.

Through its ownership model and the borrow-checker, Rust guarantees memory safety without the use of a garbage collector. Rust has consistently been ranked as the most loved programming language on the
[stackoverflow developer survey](https://survey.stackoverflow.co/2022/#section-most-loved-dreaded-and-wanted-programming-scripting-and-markup-languages) since 2016.
<!--more-->

Rust is a relatively new systems programming language that came out of Mozilla Research. Since its 1.0 release in 2015, Rust has steadily gained a lot of support and admiration, with some big tech companies investing heavily in the language.
The founding members of the [Rust Foundation](https://foundation.rust-lang.org/)
include tech heavyweights like Google, Microsoft, Meta (formerly Facebook), and AWS.
And the Linux kernel has finally taken steps to
[add support for Rust](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=8aebac82933ff1a7c8eede18cab11e1115e2062b).

Going by the current trend, Rust is definitely poised to become one of the powerhouses
in the software engineering space. If you've ever wanted to get started with Rust but were wondering how or where to find resources, you're in the right place.

This is a comprehensive guide on how to get started with the
[Rust programming language](https://www.rust-lang.org/). I hope it sets you on a steady path to mastering the language and serves as a strong starting point for your Rust journey.

>**Note**: Rust has a reputation for having a (relatively) steep learning curve. This guide assumes that you are at least familiar with programming in general (perhaps you've worked with other languages like Python or JavaScript before). Rust being a systems programming language, having _some_ experience with C/C++ would put you in a better place when starting with Rust (but it's not a _hard_ requirement). Some familiarity with functional programming would also help (but again, it's not a _hard_ requirement).

With that out of the way, let's get started.

## Installing Rust and Cargo

The [recommended way](https://www.rust-lang.org/tools/install) of installing Rust
is via [Rustup](https://rustup.rs/), which is both an installer and a version management tool.
Apart from installing Rust, Rustup also installs Cargo, the Rust build tool and package
manager (Cargo is similar to pip or npm, but much more powerful).

You can confirm that your installation is successful by running the following commands
in your terminal (or command prompt on Windows):

```bash
$ rustc --version
```

This should return the version of the Rust compiler installed.

```bash
$ cargo --version
```

This should return the version of the Cargo tool installed.

Rust has a 6-week release process (which means there's a new Rust stable release every 6 weeks).
Whenever a new version of Rust is released, you can run the command below to update
the Rust version (and the entire Rust toolchain).

```bash
$ rustup update
```

## Hello world in Rust

Let's see how we can create the traditional "Hello world" program in Rust.

In the terminal, run the following Cargo command to create a new binary project
(assuming that you've navigated to your preferred project directory):

```bash
$ cargo new hello-world --bin
```

This will generate a new directory called `hello-world` with the following file structure:

```bash
$ cd hello-world

$ tree .
├── Cargo.toml
└── src
    └── main.rs
```

`Cargo.toml` is the manifest file for a Rust project. It’s where you keep the metadata for your project, as well as your project dependencies.

`src/main.rs` is where you write your application code. The `main.rs` file has the following contents:

```rust
fn main() {
  println!("Hello, world!");
}         
```

All rust programs have a `main` function that acts as the entry point for the program.

To run the program, execute the `cargo run` command:

```bash
$ cargo run
```

The `cargo run` command builds and runs the program, printing `Hello, world!` as shown below:

```bash
$ cargo run
   Compiling hello-world v0.1.0
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/hello-world`
Hello, world!
```

Alternatively, you can build the project first using `cargo build` and then run it using `cargo run`.

Other useful Cargo commands are:
- `cargo check`: checks to ensure that your code compiles without producing an executable
(this is handy when you are making changes to your code and you need to make sure that
  it still passes the compiler checks)
- `cargo test`: runs unit and integration tests for your project
- `cargo doc`: generates HTML documentation for your project (these are generated
  from documentation comments - comments that start with three slashes, `///`)
- `cargo add`: a convenient command for adding project dependencies (which get
  automatically added to the `Cargo.toml` file)

For more on the Cargo tool, check out the [Cargo book](https://doc.rust-lang.org/cargo/index.html).

**Note**:

- When creating a Rust binary project, passing the `--bin` flag is optional: the `cargo new` command creates a binary (executable) project by default.
- But if you want to create a library project, then you need to pass the `--lib` flag to the `cargo new` command, like so:

```bash
$ cargo new library-project --lib
```
- You can also run `cargo init` in an empty directory to create a new Rust project as well, passing `--bin` or `--lib` flag as appropriate.

## Rust tooling

The Rust ecosystem provides some great set of tools for working with the language.

### Rustfmt

This tool automatically formats Rust code for you (similar to `gofmt` for Golang).
If you install Rust using the `rustup` tool, you should have `rustfmt` installed as well.
If not, then you can install it as follows:

```bash
$ rustup component add rustfmt
```

You can then run the tool on a cargo project using the following cargo command:

```bash
$ cargo fmt
```

### Clippy

This is a linting tool that helps catch common mistakes and improve your Rust code.
It ensures that you write idiomatic code that adheres to Rust's standards.

Similar to `rustfmt`, if you install Rust using the `rustup` tool, you should
have `clippy` installed too. If not, then you can install it as follows:

```bash
$ rustup component add clippy
```

You can then run the tool on a cargo project using the following cargo command:

```bash
$ cargo clippy
```

To automatically apply Clippy suggestions, run:

```bash
$ cargo clippy --fix
```

## Code Editors

A number of popular code editors/IDEs already offer great Rust support (mainly through extensions/plugins).

### VS Code + Rust Analyzer

Through the [rust-analyzer](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer) extension, [VS Code](https://code.visualstudio.com/) offers great support for Rust development. In my opinion, I think the combination of VS Code and rust-analyzer offers the best Rust development experience at the moment (although things can easily change in the future).

### IntelliJ IDEA + Rust plugin

If you are a fan of JetBrains products, there is a [Rust plugin](https://plugins.jetbrains.com/plugin/8182-rust) for [IntelliJ IDEA](https://www.jetbrains.com/idea/) (the community edition works just fine).
> Update: JetBrains now has an official Rust IDE: [RustRover](https://www.jetbrains.com/rust/).

### Neovim

[Neovim](https://neovim.io/) is a Vim-based text editor that has gained [affection from developers](https://survey.stackoverflow.co/2022/#section-most-loved-dreaded-and-wanted-integrated-development-environment) in recent years. Check out [using Neovim for Rust development](https://rusty-ferris.pages.dev/blog/using-nvim-for-rust-development/) to get started.

## Resources for Learning Rust

### "The Rust Book"

The official [Rust Programming Language book](https://doc.rust-lang.org/book/) is a great
resource for learning Rust from the ground up. As you go through the book, you also
get to build three projects (of increasing complexity), that really reinforce your
knowledge of the language. I'd recommend going through the first nine chapters to
get a proper feel of what Rust is all about. I'd also advise that you pay special
attention to chapter 4 (**Understanding ownership**), chapter 6 (**Enums and Pattern Matching**),
and chapter 9 (**Error Handling**). You may be tempted to ask me why these specific chapters.
They cover some of the (most) challenging features of Rust.

### Rust by Example

[Rust by Example](https://doc.rust-lang.org/stable/rust-by-example/) (RBE) is a great
resource for folks who like working through examples while learning a new language.
RBE offers a collection of runnable examples that illustrate various Rust concepts
and standard library features.

### Rustlings

[Rustlings](https://github.com/rust-lang/rustlings/) is a series of small exercises to get you used to reading and writing Rust code. It comes with a `rustlings` CLI tool that helps in navigating through the exercises from the command line.

If you prefer working through the exercises in an IDE environment, JetBrains offers an adaptation of the Rustlings exercises via the [Rustlings plugin](https://plugins.jetbrains.com/plugin/16631-rustlings). This requires that you install the [EduTools](https://plugins.jetbrains.com/plugin/10081-edutools) plugin as well.

I highly recommend that after going through "[the Rust book](https://doc.rust-lang.org/book/)", you should consider working through the Rustlings exercises as a next step.

### Rust Cookbook

The [Rust Cookbook](https://rust-lang-nursery.github.io/rust-cookbook/) offers a collection
of simple Rust code examples that demonstrate good practices to accomplish common
programming tasks (for specific use-cases: from cryptography to text processing and network applications).

### Rust Cheat Sheet

The [Rust Cheat Sheet](https://cheats.rs/) is a great reference material for some of the important features of the Rust language. It also has a PDF version that you can print and use offline.

### Rust track on Exercism

When you start feeling comfortable with Rust, you can try your hand on the [Rust track on Exercism](https://exercism.org/tracks/rust). This is a great way of testing your level of understanding of Rust by working through interview-style coding exercises. After submitting your own solutions, you can check out the solutions from other Rustaceans and request for mentorship as well.

### Rust Playground (or Rust Explorer)

The [Rust Playground](https://play.rust-lang.org/) gives you a platform to play with Rust without installing any of the necessary tools on your local machine. It's a great place to build prototypes quickly. It also allows you to easily share code snippets with others.

I recently learnt about a somewhat similar platform: [Rust Explorer](https://www.rustexplorer.com/b). I haven't really played with it much, but it looks like it can be used as an alternative to the Rust Playground.

## Rust Books

Apart from the official [Rust Book](https://doc.rust-lang.org/book/) mentioned above, here are other books that I've found to be useful when learning Rust:

- [Programming Rust](https://www.oreilly.com/library/view/programming-rust-2nd/9781492052586/): Now in it's second edition, it's a great reference book on Rust (it goes a lot more deeper on many of the Rust concepts, compared to "the Book").
- [Command-Line Rust](https://www.oreilly.com/library/view/command-line-rust/9781098109424/): The book takes a project-based approach to learning Rust. You get to rewrite some of the most common Unix command line utilities in Rust.
- [Rust in Action](https://www.manning.com/books/rust-in-action): Covers low-level systems programming concepts using Rust. For example, you get to write a small OS kernel and a key-value store.
- [Zero to Production in Rust](https://www.zero2prod.com/index.html): A fantastic introduction to backend (API) development using Rust. The book uses the [Actix web](https://actix.rs/) framework.
- [Rust for Rustaceans](https://nostarch.com/rust-rustaceans): This book targets an intermediate-level audience (at least you should have a good grasp of Rust fundamentals before getting this book). The author indicates that the book is meant to pick up from where "the Book" left off. It covers some advanced topics like FFI, Unsafe Code, and Macros.

## Rust-focused YouTube Channels

Here are a few YouTube channels that you might find useful (if you are a fan of YouTube videos):

- [Let's Get Rusty](https://www.youtube.com/c/LetsGetRusty): A great channel for Rust beginners.
- [Ryan Levick](https://www.youtube.com/c/RyanLevicksVideos): Another great channel for Rust beginners. Ryan is a member of the Rust core team.
- [Jon Gjengset](https://www.youtube.com/c/JonGjengset): Jon is the author of the _Rust for Rustaceans_ book. His videos tend to be pretty long, most of which are usually streamed live on Twitch.
- [Tim McNamara](https://www.youtube.com/c/timClicks): Tim is the author of the _Rust in Action_ book.

## Rust Courses

If you like taking structured courses, then here are some of my favorites:

- [Take your first steps with Rust](https://learn.microsoft.com/en-us/training/paths/rust-first-steps/): This is a free 5-hour Learning Path on Microsoft Learn. The content is beginner-friendly and covers most of Rust basics.
- [Rust Essential Training](https://www.linkedin.com/learning/rust-essential-training): This is a course on LinkedIn Learning (Subscription needed).
- [Rust Fundamentals](https://www.pluralsight.com/courses/fundamentals-rust): This is a course on Pluralsight (Subscription needed).
- [Comprehensive Rust](https://google.github.io/comprehensive-rust/): A Rust course by the Google Android team.

## The Rust Community

If you'd like to interact with fellow Rustaceans within the wider Rust ecosystem, here are a few places to look at. Rust is known to have a very welcoming (and inclusive) community.

- Rust [users forum](https://users.rust-lang.org/): An open forum for Rustaceans. You can ask for help or discuss any topic related to Rust.
- Rust [Discord server](https://discord.gg/rust-lang): Get to chat and interact in real time with other Rustaceans. Folks are pretty active (and responsive) on this server.
- Rust [subreddit](https://www.reddit.com/r/rust/): Another great place to learn about the latest news on Rust and its ecosystem.
- [This Week in Rust](https://this-week-in-rust.org/) newsletter: Weekly Rust ecosystem updates delivered via email (I've personally learned a lot from this newsletter)
- [Awesome Rust Mentors](https://rustbeginners.github.io/awesome-rust-mentors/): Ask for mentorship from more experienced Rustaceans.

## Conclusion

Rust is rapidly gaining popularity. Even the folks at WIRED think that Rust is [taking over tech](https://www.wired.com/story/rust-secure-programming-language-memory-safe/). I hope that this guide helps you in your journey to becoming a productive Rust developer.

May the (Ferris) force be with you!
