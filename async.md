## rust
Note: all this is from the [rust async book](https://rust-lang.github.io/async-book).


the idea of writing async code is perform different tasks asynchronously. In Rust, we have a the `async fn` keyword which returns a `Future`. To execute the body of the function the Future must be run to completion.

the `async` keyword transforms a block of code into a state machine that implements the `Future` trait.
