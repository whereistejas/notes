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
This PR is ready from my side.

Tasks:
- [x] Add tests.
- [x] Feature gate and make unstable.
- [x] Update `cargo-feature` documentation.
- [x] Add appropriate comments where needed.

This is a short description of my implementation:
1. We are receiving the name the user expects to give the binary in a parameter called `filename` from `cargo.toml`.
2. This value is stored in the `bin_name` of the `TargetInner` struct.
3. We use this name to to create the `hardlink`, in the target directory using `uplift_filename` method. The other binaries that are created in the `deps/` folder, have the same name as the crate.
4. The `uplift_filename` method is used in three places:
i. `fn clean` - used for cargo clean.
https://github.com/rust-lang/cargo/blob/54bc19ee56c2251fc749198cce9605450fecd1df/src/cargo/ops/cargo_clean.rs#L190-L197
ii. `fn bin_link_for_target` - this method is only used when no `Unit` is found.
https://github.com/rust-lang/cargo/blob/54bc19ee56c2251fc749198cce9605450fecd1df/src/cargo/core/compiler/context/compilation_files.rs#L304-L307
We can safely assume that `uplift_filename` here will only return the
iii. `fn uplift_to` - used for cargo build, this is our method of interest.

### `--message-format=json`
>     * Check the JSON output for the artifact message in `--message-format=json` contains the correct path.

Something seems to be wrong, here. If you look at the output produced
```
{"reason":"compiler-artifact",
"package_id":"foo 0.0.1 (path+file:///D:/a/cargo/cargo/target/debug/tmp/cit/t160/foo)",
"manifest_path":"D://a//cargo//cargo//target//debug//tmp//cit//t160//foo//Cargo.toml",
"target":{
 "kind":["bin"],
 "crate_types":["bin"],
 "name":"foo",
 "src_path":"D://a//cargo//cargo//target//debug//tmp//cit//t160//foo//src/main.rs",
 "edition":"2015",
 "doc":true,
 "doctest":false,
 "test":true},
"profile":{"opt_level":"0","debuginfo":2,"debug_assertions":true,"overflow_checks":true,"test":false},
"features":[],
"filenames":["D://a//cargo//cargo//target//debug//tmp//cit//t160//foo//target//debug//007bar.exe","D://a//cargo//cargo//target//debug//tmp//cit//t160//foo//target//debug//007bar.pdb"],
"executable":"D://a//cargo//cargo//target//debug//tmp//cit//t160//foo//target//debug//007bar.exe","fresh":false}
 ```
 Shouldn't all the paths in Windows have forward slashes? Am I missing something here?
