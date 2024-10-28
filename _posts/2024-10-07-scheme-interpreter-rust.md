---
layout: post
title: "Building a minimal Scheme Interpreter in Rust"
description: "A tutorial on to write a minimal Lisp-like interpreter in Rust"
comments: true
tags: Rust Scheme Lisp Funtional programming S-expressions
excerpt_separator: <!--more-->
---

One of my main technical interests is programming languages and compilers. I have read a decent amount of material on the subject. Recently, I decided to put my knowledge into practice by building a simple language interpreter.

By the end of this guide we'll have a minimal, working interpreter for a small subset of the 
[Scheme](https://en.wikipedia.org/wiki/Scheme_(programming_language)) programming language.
<!--more-->

> Note: This guide assumes that you're relatively comfortable with Rust.

All source code for this guide is [available on GitHub](https://github.com/chrischiedo/rustyscm).


## What is Scheme?

Scheme is a dialect of the [Lisp](https://en.wikipedia.org/wiki/Lisp_(programming_language)) programming language. In Scheme (just like in other Lisp dialects), programs consist purely of _expressions_. These expressions are technically known as [S-Expressions](https://en.wikipedia.org/wiki/S-expression).

An S-Expression (Symbolic Expression) is basically a _list_ wrapped in parentheses with spaces between elements:

```scheme
> (+ 1 2 3)
```

This is why everything in Lisp is usually considered to be a list. In fact, the name Lisp is an acronym for **LIS**t **P**rocessing.

Other Lisp dialects are [Common Lisp](https://lisp-lang.org/), [Racket](https://racket-lang.org/), and [Clojure](https://clojure.org/).


## Syntax and Semantics of A Scheme Program

Scheme syntax is much simpler compared to most other programming languages:

- As we've already mentioned, Scheme programs consist solely of _expressions_. There is no distinction between a statement and an expression.
- Numbers (e.g. 10) and symbols (e.g. X) are called _atomic expressions_; they cannot be broken into smaller pieces. In Scheme, operators such as +, /, and > are _symbols_ too.
- Everything else is a _list expression_, i.e. an "(", followed by zero or more expressions, followed by a ")". The first element of the list determines its syntactic meaning:
	- A list starting with a keyword, e.g. (_if_ ...), is a _special form_; the meaning depends on the keyword.
	- A list starting with a non-keyword, e.g. (_fn_ ...), is a _function call_.

You can read more about the syntax rules of Scheme programs from Peter Norvig's excellent [article](https://norvig.com/lispy.html).

## The Structure of A Language Interpreter

At a high level, a language interpreter has the following parts:

![Language Interpreter stages](/assets/img/scheme-interpreter-rust/interpreter-pipeline.png "Language Interpreter stages")

- The lexer (sometimes called a scanner), takes in the program source code and breaks it up into a series of _tokens_.
- The parser takes in tokens from the lexer and creates an abstract syntax tree (AST).
- The evaluator evaluates the AST and produces the final result of the program.


With the preliminaries out of the way, let's get started.

This guide is divided into seven steps:

* Step 1 - Initialize a new project
* Step 2 - Lexical Analysis
* Step 3 - Parsing
* Step 4 - Evaluation
* Step 5 - Create a REPL
* Step 6 - Run the program
* Step 7 - Add tests


## Step 1: Initialize a new project

Create a new Rust project by running the following in your shell:

```bash
$ cargo new rustyscm
$ cd rustyscm
```

The `rustyscm` directory will have the following file structure:

```bash
.
├── Cargo.lock
├── Cargo.toml
└── src
    └── main.rs
```

## Step 2: Lexical Analysis

Lexical analysis breaks up a program's source code into a sequence of tokens. In the small subset of Scheme that we are building, we have the following tokens:

- Parentheses: "(" and ")"
- Numbers: 64-bit floating-point numbers
- Symbols: Any group of characters other than a number or parenthesis

In Rust, we'll represent the tokens with an enum:

File: `src/lexer.rs`
{:.muted}

```rust
#[derive(Debug, Clone, PartialEq)]
pub enum Token {
    OpenParen,
    CloseParen,
    Number(f64),
    Symbol(String),
}
```

We also implement the `Display` trait for `Token`:

File: `src/lexer.rs`
{:.muted}

```rust
impl std::fmt::Display for Token {
    fn fmt(&self, f: &mut std::fmt::Formatter) -> std::fmt::Result {
        match self {
            Token::OpenParen => write!(f, "("),
            Token::CloseParen => write!(f, ")"),
            Token::Number(n) => write!(f, "{}", n),
            Token::Symbol(s) => write!(f, "{}", s),
        }
    }
}
```

The main process of tokenization is done by the _tokenize_ function:

File: `src/lexer.rs`
{:.muted}

```rust
pub fn tokenize(expr: &str) -> Result<Vec<Token>, String> {
    let expr = expr.replace("(", " ( ").replace(")", " ) "); // add spaces around each paren

    let words = expr.split_whitespace(); // delimit characters by whitespace

    let mut tokens: Vec<Token> = Vec::new();

    // convert words into tokens using pattern matching and add to the vector
    for word in words {
        match word {
            "(" => tokens.push(Token::OpenParen),
            ")" => tokens.push(Token::CloseParen),
            _ => {
                let i = word.parse::<f64>();
                if i.is_ok() {
                    tokens.push(Token::Number(i.unwrap()));
                } else {
                    tokens.push(Token::Symbol(word.to_string()));
                }
            }
        }
    }

    Ok(tokens)
}
```

The tokenize funtion takes as input an expression (a string of characters) and returns a vector of tokens.

To see how the lexer works, add the following code to the `main` file:

File: `src/main.rs`
{:.muted}

```rust
mod lexer;

use std::io;

use crate::lexer::tokenize;

fn main() {
    let mut input = String::new();

    io::stdin()
        .read_line(&mut input)
        .expect("Failed to read line");

    let tokens = tokenize(input.as_str()).unwrap();

    println!("Tokens: {:?}", tokens);
}
```

Now if you run the program using:

```bash
$ cargo run
```

You'll get a blinking cursor waiting for you to type a Scheme expression. After typing an expression, you'll get an output similar to the one shown below:

```bash
> (+ 1 2 3)
> Tokens: [OpenParen, Symbol("+"), Number(1.0), Number(2.0), Number(3.0), CloseParen]
```

As you can see the input string is split into a vector of tokens.

## Step 3: Parsing

Parsing (or _syntactic analysis_) converts the tokens from the lexer into an abstract syntax tree. In our particular case, the parser will convert the tokens into a list of Scheme expressions that can be interpreted by the evaluator.

Here's the enum representing valid Scheme expressions:

File: `src/parser.rs`
{:.muted}

```rust
#[derive(Debug, Clone, PartialEq)]
pub enum Expression {
    Bool(bool),
    Number(f64),
    Symbol(String),
    List(Vec<Expression>),
    Func(fn(&[Expression]) -> Expression),
    Function(Procedure),
}
```

We also need to implement the `Display` trait for `Expression`:

File: `src/parser.rs`
{:.muted}

```rust
impl std::fmt::Display for Expression {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        match self {
            Expression::Bool(a) => write!(f, "{}", a),
            Expression::Number(n) => write!(f, "{}", n),
            Expression::Symbol(s) => write!(f, "{}", s),
            Expression::List(list) => {
                let formatted_list: Vec<String> =
                    list.iter().map(|exp| format!("{}", exp)).collect();
                write!(f, "({})", formatted_list.join(" "))
            }
            Expression::Func(_) => write!(f, "<function>"),
            Expression::Function(_) => write!(f, "<function>"),
        }
    }
}
```

And here's the `Procedure` struct. This represents a Scheme function/procedure:

File: `src/parser.rs`
{:.muted}

```rust
#[derive(Debug, Clone, PartialEq)]
pub struct Procedure {
    pub params: Vec<Expression>,
    pub body: Vec<Expression>,
    pub env: Environment,
}
```

From the struct definition above, we see that a procedure has parameters, its body and its execution environment.

### What is an Environment?

In Scheme, an _environment_ is a mapping from variable names to their values. In Rust, we can represent an environment using a HashMap:

File: `src/env.rs`
{:.muted}

```rust
use std::collections::HashMap;

use crate::parser::Expression;

#[derive(Debug, Clone, PartialEq)]
pub struct Environment {
    contents: HashMap<String, Expression>,
}
```

As you can see, the `Environment` type is just a wrapper around a Rust HashMap.

We also need to define a few functionalities for the Environment:

File: `src/env.rs`
{:.muted}

```rust
impl Environment {
    fn new() -> Self {
        Self {
            contents: HashMap::new(),
        }
    }

    pub fn insert(&mut self, k: String, v: Expression) {
        self.contents.insert(k, v);
    }

    pub fn get(&self, k: &str) -> Option<&Expression> {
        self.contents.get(k)
    }
}
```

Back to parsing, this is how the `parse` function looks like:

File: `src/parser.rs`
{:.muted}

```rust
pub fn parse(input: &str) -> Result<Expression, String> {
    let token_result = match tokenize(input) {
        Ok(val) => val,
        Err(err) => return Err(format!("{}", err)),
    };

    let mut tokens = token_result.into_iter().rev().collect(); // need to reverse the tokens from the lexer

    parse_token_list(&mut tokens)
}
```

The real work happens in the recursive `parse_token_list` function:

File: `src/parser.rs`
{:.muted}

```rust
fn parse_token_list(tokens: &mut Vec<Token>) -> Result<Expression, String> {
    let token = tokens.pop();

    if token != Some(Token::OpenParen) {
        return Err(format!("Error: Expected OpenParen, found {:?}", token));
    }

    let mut list: Vec<Expression> = Vec::new();

    while !tokens.is_empty() {
        let token = tokens.pop();

        if token == None {
            return Err("Error: Did not find enough tokens".to_string());
        }

        let tok = token.unwrap();

        match tok {
            Token::Number(n) => list.push(Expression::Number(n)),
            Token::Symbol(s) => list.push(Expression::Symbol(s)),
            Token::OpenParen => {
                tokens.push(Token::OpenParen);
                let sub_list = parse_token_list(tokens)?; // recursive call
                list.push(sub_list);
            }
            Token::CloseParen => {
                return Ok(Expression::List(list));
            }
        }
    }

    Ok(Expression::List(list))
}
```

The `parse_token_list` function takes a vector of tokens (in reverse order) generated by the lexer and generates a single _List Expression_ representing the entire Scheme program. The function expects a vector of tokens, with the first token of the vector being an open parenthesis indicating the start of the list. The function then proceeds to process the elements of the list one at a time. If it encounters atomic tokens such as a number or symbol, it creates corresponding atomic expressions and adds them to the list. If it encounters another open parenthesis it recurses with the remaining tokens. Finally, the function returns with the list expression when it encounters the close parenthesis.

## Step 4: Evaluation

Evaluation is the final step that will produce the result for the Scheme program. At a high level, the evaluator walks the List-based structure created by the parser and evaluates each atomic expression and list (recursively), combines these intermediate values, and produces the final result.

By default, the evaluator will use a global environment that includes the names of a small set of standard operations (e.g. +, -, and _pow_). This environment can be augmented with user-defined variables, using the expression: _(define symbol value)_.

This is how we can define an environment with some Scheme standard operations:

File: `src/env.rs`
{:.muted}

```rust
use std::f64::consts::PI;

use crate::operator_utils::*;

pub fn standard_env() -> Environment {
    let mut environment = Environment::new();

    // Basic arithmetic operators
    environment.insert(
        "+".to_string(),
        Expression::Func(|args: &[Expression]| match add(args) {
            Ok(expr) => expr,
            Err(e) => panic!("{}", e),
        }),
    );
    environment.insert(
        "-".to_string(),
        Expression::Func(|args: &[Expression]| match subtract(args) {
            Ok(expr) => expr,
            Err(e) => panic!("{}", e),
        }),
    );
    environment.insert(
        "*".to_string(),
        Expression::Func(|args: &[Expression]| match multiply(args) {
            Ok(expr) => expr,
            Err(e) => panic!("{}", e),
        }),
    );
    environment.insert(
        "/".to_string(),
        Expression::Func(|args: &[Expression]| match divide(args) {
            Ok(expr) => expr,
            Err(e) => panic!("{}", e),
        }),
    );

    // Exponent
    environment.insert(
        "pow".to_string(),
        Expression::Func(|args: &[Expression]| match power(args) {
            Ok(expr) => expr,
            Err(e) => panic!("{}", e),
        }),
    );

    // PI constant
    environment.insert("pi".to_string(), Expression::Number(PI));

    environment
}
```

The standard environment uses a bunch of operator utility functions defined in the 
`operator_utils` module.

Here's one example of such a utility function for calculating exponent:

File: `src/operator_utils.rs`
{:.muted}

```rust
use crate::parser::Expression;

use anyhow::{anyhow, Result};

pub fn power(args: &[Expression]) -> Result<Expression> {
    if args.len() != 2 {
        return Err(anyhow!(
            "Exponent operator requires two arguments: base and exponent factor"
        ));
    }

    let base = match args.get(0) {
        Some(Expression::Number(num)) => Ok(num),
        _ => Err(anyhow!("Expected a number")),
    }?;

    let n = match args.get(1) {
        Some(Expression::Number(num)) => Ok(num),
        _ => Err(anyhow!("Expected a number")),
    }?;

    let result = base.powf(*n);

    Ok(Expression::Number(result))
}
```

> Note: Please check the project's GitHub [repo](https://github.com/chrischiedo/rustyscm) for the rest of the utility functions.

Here's the `eval` function, which represents the entry point to the evaluation process:

File: `src/eval.rs`
{:.muted}

```rust
pub fn eval(program: &str, env: &mut Environment) -> Result<Expression, String> {
    let parsed_expr = match parse(program) {
        Ok(expr) => expr,
        Err(error) => {
            eprintln!("Error during parsing: {}", error);
            std::process::exit(1);
        }
    };

    let evaluated_expression = eval_expr(parsed_expr, env)?;

    Ok(evaluated_expression)
}
```

The `eval` function calls the `eval_expr` function shown below:

File: `src/eval.rs`
{:.muted}

```rust
fn eval_expr(expr: Expression, env: &mut Environment) -> Result<Expression, String> {
    match expr {
        Expression::Bool(_) => Ok(expr),
        Expression::Symbol(s) => env
            .get(&s)
            .cloned()
            .ok_or_else(|| format!("Undefined symbol: {}", s)),
        Expression::Number(_) => Ok(expr),
        Expression::List(list) => eval_list(&list, env),
        Expression::Func(_) => Ok(expr),
        Expression::Function(_) => Err("Unexpected function definition".into()),
    }
}
```

The bulk of the evaluation process happens in the `eval_list` function. The function takes the List expression representing the program and the environment variable as inputs. The function then starts processing the List expression representing the program by iterating over each element of the list:

File: `src/eval.rs`
{:.muted}

```rust
use crate::env::Environment;

use crate::parser::Expression;

fn eval_list(list: &[Expression], env: &mut Environment) -> Result<Expression, String> {
    let first = &list[0];

    if let Expression::Symbol(s) = first {
        match s.as_str() {
            "define" => eval_define(&list, env),
            "if" => eval_if(&list, env),
            _ => {
                if let Some(exp) = env.get(s) {
                    match exp {
                        Expression::Func(f) => {
                            let function = f.clone();
                            let args: Result<Vec<Expression>, String> = list[1..]
                                .iter()
                                .map(|x| eval_expr(x.clone(), env))
                                .collect();
                            Ok(function(&args?))
                        }
                        Expression::Function(proc) => {
                            let env_clone = &mut env.clone();

                            let args: Result<Vec<Expression>, String> = list[1..]
                                .iter()
                                .map(|x| eval_expr(x.clone(), env_clone))
                                .collect();

                            // Create a new execution environment for the function
                            let mut local_env = proc.env.clone();

                            // Insert the function name into the new environment
                            local_env.insert(s.clone(), exp.clone());

                            for (param, arg) in proc.params.iter().zip(args?) {
                                if let Expression::Symbol(param_name) = param {
                                    local_env.insert(param_name.clone(), arg);
                                } else {
                                    return Err("Invalid parameter name".into());
                                }
                            }

                            let mut result = Expression::Bool(false);

                            for exp in proc.body.clone() {
                                result = eval_expr(exp.clone(), &mut local_env)?;
                            }

                            Ok(result)
                        }
                        _ => Err(format!("Undefined function: {}", s)),
                    }
                } else {
                    Err(format!("Undefined function: {}", s))
                }
            }
        }
    } else {
        Err("Expected a symbol".into())
    }
}
```

Here's the `eval_define` function:

File: `src/eval.rs`
{:.muted}

```rust
fn eval_define(list: &[Expression], env: &mut Environment) -> Result<Expression, String> {
    if list.len() < 3 {
        return Err("'define' requires at least two arguments".into());
    }

    // Define a new function or variable
    if let Expression::List(func) = list.get(1).unwrap() {
        if let Some(Expression::Symbol(func_name)) = func.get(0) {
            let params = func[1..].to_vec();
            let body = list.get(2..).ok_or("Invalid define syntax")?.to_vec();

            let proc = Procedure {
                params,
                body,
                env: env.clone(),
            };

            let function = Expression::Function(proc);

            env.insert(func_name.clone(), function);
            Ok(Expression::Symbol(func_name.clone()))
        } else {
            Err("Invalid define syntax".into())
        }
    } else if let Expression::Symbol(var_name) = list.get(1).unwrap() {
        let value = eval_expr(list[2].clone(), env)?;
        env.insert(var_name.clone(), value.clone());
        return Ok(Expression::Symbol(var_name.clone()));
    } else {
        return Err("Invalid define syntax".into());
    }
}
```

And here's the `eval_if` function:

File: `src/eval.rs`
{:.muted}

```rust
fn eval_if(list: &[Expression], env: &mut Environment) -> Result<Expression, String> {
    if list.len() < 4 {
        return Err("'if' requires at least three arguments".into());
    }

    let condition = eval_expr(list[1].clone(), env)?;

    match condition {
        Expression::Bool(true) => eval_expr(list[2].clone(), env),
        Expression::Bool(false) => eval_expr(list[3].clone(), env),
        _ => Err("Invalid condition in if expression".into()),
    }
}
```

At this point, we are done with all the important parts of the interpreter.

Next up, we are going to create a REPL to interact with our Scheme programs from the command line.

## Step 5: Create a REPL

In Scheme, a REPL (Read-Eval-Print-Loop) gives us a way of interacting with a program by entering an expression and seeing it immediately read, evaluated, and printed, without having to go through a lengthy build/compile/run cycle.

The REPL is implemented as a simple loop that takes one line as an input from the user, evaluates it, and prints out the evaluated result.

The `repl` function is defined in the `lib.rs` file:

File: `src/lib.rs`
{:.muted}

```rust
use std::io;

pub mod env;
pub mod eval;

use crate::env::standard_env;
use crate::eval::eval;

pub fn repl() {
    let mut global_env = standard_env();

    loop {
        println!("schemer>");

        let expr = read_input().unwrap();

        match eval(expr.as_ref(), &mut global_env) {
            Ok(val) => println!(" ==> {}", val),
            Err(error) => eprintln!("==> Error: {}", error),
        };
    }
}
```

The `repl` function uses the `read_input` helper function:

File: `src/lib.rs`
{:.muted}

```rust
use anyhow::{Context, Result};

fn read_input() -> Result<String> {
    let mut raw_input = String::new();

    io::stdin()
        .read_line(&mut raw_input)
        .context("Failed to read line")?;

    Ok(raw_input)
}
```

The `main` function just calls the `repl` function to start the program:

File: `src/main.rs`
{:.muted}

```rust
fn main() {
    rustyscm::repl();
}
```

At this point the file structure should look like this:

```bash
.
├── Cargo.lock
├── Cargo.toml
└── src
    ├── env.rs
    ├── eval.rs
    ├── lexer.rs
    ├── lib.rs
    ├── main.rs
    ├── operator_utils.rs
    └── parser.rs
```

## Step 6: Run the program

We can run the program by using:

```bash
$ cargo run
```

You'll get a prompt like this with a blinking cursor below it:

```scheme
schemer>
_
```

You can then type your Scheme expressions as follows:

```scheme
schemer>
(+ 2 2)
 ==> 4
```

Our little Scheme interpreter is capable of evaluating basic arithmetic operations:

```scheme
schemer>
(+ 2 2 3)
 ==> 7

schemer>
(- 4 2)
 ==> 2

schemer>
(+ 10 5 (- 10 3 3))
 ==> 19

schemer>
(* 3 2)
 ==> 6

schemer>
(/ 8 4)
 ==> 2

schemer>
(pow 2 16)
 ==> 65536
```

We can define variables:

```scheme
schemer>
(define x 5)
 ==> x

schemer>
(* x 2)
 ==> 10

schemer>
(define r 10)
 ==> r

schemer>
(* pi (* r r))
 ==> 314.1592653589793
```

We can do comparison:

```scheme
schemer>
(< 4 2)
 ==> false

schemer>
(> 4 2)
 ==> true

schemer>
(>= 4 2)
 ==> true
```

We can evaluate `if` expressions:

```scheme
schemer>
(if (> 2 4 ) 1 2)
 ==> 2

schemer>
(if (< 2 4 ) 1 2)
 ==> 1

schemer>
(if (> (* 11 11) 120) (* 7 6) oops)
 ==> 42
```

We can define functions:

```scheme
schemer>
(define (sum a b) (+ a b))
 ==> sum

schemer>
(sum 10 20)
 ==> 30

schemer>
(define (square x) (* x x))
 ==> square

schemer>
(square 5)
 ==> 25

schemer>
(define (circle-area r) (* pi (* r r)))
 ==> circle-area

schemer>
(circle-area (+ 5 5))
 ==> 314.1592653589793

schemer>
(circle-area 3)
 ==> 28.274333882308138
```

We can define functions to calculate factorials and fibonacci numbers: 

```scheme
schemer>
(define (fact n) (if (<= n 1) 1 (* n (fact (- n 1)))))
 ==> fact

schemer>
(fact 10)
 ==> 3628800

schemer>
(define (fib n) (if (< n 2) 1 (+ (fib (- n 1)) (fib (- n 2)))))
 ==> fib

schemer>
(fib 10)
 ==> 89
```

We can even work with nested functions:

```scheme
schemer>
(define (cube x) (define (square x) (* x x)) (* x (square x)))
 ==> cube

schemer>
(cube 3)
 ==> 27
```

We now have a complete, working interpreter for a subset of the Scheme programming language.

## Step 7: Add tests

Our final step is to add both unit and integration tests for the project.

Here's an example of a unit test for the lexer's `tokenize` function:

File: `src/lexer.rs`
{:.muted}

```rust
// ...rest of file contents omitted...

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_tokenize() {
        let input = "(define r 10)";

        let actual_tokens = tokenize(input).unwrap_or(vec![]);

        let expected_tokens = vec![
            Token::OpenParen,
            Token::Symbol("define".to_string()),
            Token::Symbol("r".to_string()),
            Token::Number(10.0),
            Token::CloseParen,
        ];

        assert_eq!(actual_tokens, expected_tokens);
    }
}
```

Integration tests in Rust are placed inside a separate `tests` directory.

Here's an example of an integration test that verifies the functionality for defining a procedure:

File: `tests/integration_test.rs`
{:.muted}

```rust
use rustyscm::env::standard_env;
use rustyscm::eval::eval;
use rustyscm::parser::Expression;

#[test]
fn test_function_definition() {
    let mut env = standard_env();

    let input1 = "(define (square x) (* x x))";
    let result1 = eval(input1, &mut env).unwrap();

    assert_eq!(result1, Expression::Symbol("square".to_string()));

    let input2 = "(square 5)";
    let result2 = eval(input2, &mut env).unwrap();

    assert_eq!(result2, Expression::Number(25.0));
}
```

The final project structure should now look like this:

```bash
.
├── Cargo.lock
├── Cargo.toml
├── src
│   ├── env.rs
│   ├── eval.rs
│   ├── lexer.rs
│   ├── lib.rs
│   ├── main.rs
│   ├── operator_utils.rs
│   └── parser.rs    
└── tests
    └── integration_test.rs

```

To run the tests use:

```bash
$ cargo test
```

## Summary

In this guide, we've been able to implement a working interpreter for a small subset of the Scheme programming language. Of course, our interpreter lacks a lot of other features that a "real" language should have. But it's good enough for our learning purposes. 

## Resources

This guide was inspired by the following excellent resources:

- [How to Write a Lisp Interpreter in Python](https://norvig.com/lispy.html)
- [Writing a minimal Lua implementation with a virtual machine from scratch in Rust](https://notes.eatonphil.com/lua-in-rust.html)
- [Crafting Interpreters](https://craftinginterpreters.com/)

