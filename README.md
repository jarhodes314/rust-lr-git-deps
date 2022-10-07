# Rust local registry with git dependencies

## Requirements

`cargo local-registry` is required, this can be installed via `cargo install cargo-local-registry`.

## How to test the cargo changes

This is an example of a maximally simple Rust crate with a git dependency cached in a local registry. Using cargo 1.63.0, running `cargo build --frozen` will fail, but with the changes in <INSERT PR LINK HERE>, it will succeed, as cargo will not try and add a checksum field to the "example" dependency. Running `cargo local-registry -s Cargo.lock registry --git` (to update the local registry) should succeed.

To see the motivation behind omitting the checksum from the registry, running `cargo build` with cargo 1.63.0 will add the checksum field into the lockfile. This will cause `cargo local-registry -s Cargo.lock registry --git` to fail. This is the primary reason why the change needs to be in cargo, rather than in cargo local-registry.
