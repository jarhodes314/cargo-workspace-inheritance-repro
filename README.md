# cargo-workspace-inheritance-repro
A reproduction of what appears to be a bug in Cargo.

Inside this crate, there are two workspaces. There is a root workspace, which
has one member `outer-workspace-crate`. There is also a workspace inside, called
`inner-workspace`, with a library, `example-lib`, inside that workspace. 

Running `cargo check` inside `inner-workspace` succeeds, but running `cargo check`
in the outer workspace does not. This is because `example-lib` determines
its package version using workspace inheritance (so it refers to the version
defined in `inner-workspace/Cargo.toml`). However, when building
`outer-workspace-crate`, `cargo` attempts to resolve the inherited version from
the root `Cargo.toml` file, which fails as no version is defined here.