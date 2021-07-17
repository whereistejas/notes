## cargo notes
we need to figure out at what point the manifest gets populated

1. the manifest is read by this method: [`do_read_manifest`](https://github.com/rust-lang/cargo/blob/3ebb5f15a940810f250b68821149387af583a79e/src/cargo/util/toml/mod.rs#L60)
2. the manifest is converted into a `Target` structure by this method: [`to_real_manifest`](https://github.com/rust-lang/cargo/blob/3ebb5f15a940810f250b68821149387af583a79e/src/cargo/util/toml/mod.rs#L1032) :
	1. we fill the `Target` struct for `bin` type [here](https://github.com/rust-lang/cargo/blob/3ebb5f15a940810f250b68821149387af583a79e/src/cargo/util/toml/targets.rs#L63).

## uplift_filename
this method is used in three places:
1. `fn bin_link_for_target` - this method is only used when we don't have an `Unit`.
2. `fn clean` - in the file `src/cargo/ops/cargo_clean.rs`.
3. `fn uplift_to` - this is the method we are interested in.

## final pr review
Hi,
I think the PR is pretty much ready at this point. I will outline the implementation details:
1. Our goal is to allow setting of the binary name, without breaking any of the other features such as `cargo clean` or the `deps` file.
