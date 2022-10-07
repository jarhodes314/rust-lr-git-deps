# Rust local registry with git dependencies

## Requirements

A patched version of `cargo local-registry` is required, this can be installed via `cargo install --git https://github.com/jarhodes314/cargo-local-registry --branch feature/null-checksums`.

## How to test the cargo changes

This is an example of a maximally simple Rust crate with a git dependency cached in a local registry. Using cargo 1.63.0, running `cargo build --frozen` will fail, but with the changes in https://github.com/rust-lang/cargo/pull/11188, it will succeed, as cargo will not try and add a checksum field to the "example" dependency. Running `cargo local-registry -s Cargo.lock registry --git` (to update the local registry) should succeed.

To see the motivation behind omitting the checksum from the registry rather than calculating a fake one in `cargo local-registry`, modify the registry index for example to contain `"cksum":""` instead of `"cksum":null`. Running `cargo build` with cargo 1.63.0 will add the checksum field into the lockfile. This will cause `cargo local-registry -s Cargo.lock registry --git` to fail. This is the primary reason why the change needs to be in cargo, rather than in cargo local-registry.
