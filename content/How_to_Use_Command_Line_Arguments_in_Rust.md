---
title: "How to Use Command Line Arguments in Rust"
description: "Learn how to handle command line arguments in Rust using std::env and the clap crate, with practical examples."
tags: ["Rust", "Command Line Arguments", "Programming", "CLI", "Rust CLI", "Terminal"]
---

# How to Use Command Line Arguments in Rust

Command line arguments are a powerful way to influence the behavior of your programs at runtime. In Rust, handling command line arguments is straightforward and can be done using the `std::env` module or the `clap` crate for more advanced parsing. This guide will walk you through both methods with examples.

## Using `std::env` Module

The `std::env` module provides functions to interact with the operating system’s environment. You can retrieve the command line arguments using the `args` function, which returns an iterator.

### Basic Example with `std::env::args`

Here’s a simple example that prints the command line arguments:

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    
    println!("Number of arguments: {}", args.len());
    for (i, argument) in args.iter().enumerate() {
        println!("Argument {}: {}", i, argument);
    }
}
```

To run this program:

```sh
cargo run -- arg1 arg2 arg3
```

Output:

```
Number of arguments: 4
Argument 0: target/debug/my_program
Argument 1: arg1
Argument 2: arg2
Argument 3: arg3
```

### Parsing Arguments

Often, you’ll need to parse command line arguments to specific types. Here’s an example of parsing two integers and summing them:

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    
    if args.len() != 3 {
        eprintln!("Usage: {} <num1> <num2>", args[0]);
        std::process::exit(1);
    }
    
    let num1: i32 = args[1].parse().expect("Please provide a valid integer");
    let num2: i32 = args[2].parse().expect("Please provide a valid integer");

    println!("The sum is: {}", num1 + num2);
}
```

## Using `clap` Crate

For more complex command line argument parsing, the `clap` crate is a popular choice. It provides a declarative way to specify arguments, options, and subcommands.

### Basic Example with `clap`

First, add `clap` to your `Cargo.toml`:

```toml
[dependencies]
clap = "4.0"
```

Here’s an example that uses `clap` to parse a name and an optional age:

```rust
use clap::{Arg, Command};

fn main() {
    let matches = Command::new("my_program")
        .version("1.0")
        .author("Your Name <your.email@example.com>")
        .about("Example program with clap")
        .arg(Arg::new("name")
            .about("Sets the name to use")
            .required(true)
            .index(1))
        .arg(Arg::new("age")
            .about("Sets the age to use")
            .required(false)
            .index(2))
        .get_matches();

    let name = matches.value_of("name").unwrap();
    let age = matches.value_of("age").unwrap_or("unknown");

    println!("Name: {}", name);
    println!("Age: {}", age);
}
```

To run this program:

```sh
cargo run -- John 30
```

Output:

```
Name: John
Age: 30
```

If you omit the age:

```sh
cargo run -- John
```

Output:

```
Name: John
Age: unknown
```

## Conclusion

Handling command line arguments in Rust can be as simple or as complex as your application requires. For basic needs, the `std::env` module is sufficient. For more advanced scenarios, the `clap` crate offers extensive features and ease of use. By leveraging these tools, you can create flexible and user-friendly command line applications in Rust.
