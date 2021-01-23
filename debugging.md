## `rust` debugging

Steps using gdb:
1. `cargo build <source-code>`
2. `gdbserver localhost:9000 target/path-to-executable`
3. `gdbgui` and connect to `gdbserver` address
4. set breakpoint using `break source-code.rs:line-number`
5. n -> next, s -> step, fin -> finish, c-> continue, print variable

youtube videos:
1. [how to use gdbserver](https://www.youtube.com/watch?v=WiRgl2qq5Sc)
2. [how to use gdb](https://www.youtube.com/watch?v=8D74GaBIYI4)

