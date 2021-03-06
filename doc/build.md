# Grin - Build, Configuration, and Running

## Supported Platforms

Longer term, most platforms will likely be supported to some extent.
Grin's programming language `rust` has build targets for most platforms.

What's working so far?
* Linux x86\_64 and MacOS [grin + mining + development]
* Not Windows 10 yet [grin kind-of builds. No mining yet. Help wanted!]

## Requirements

- rust 1.26+ (use [rustup]((https://www.rustup.rs/))- i.e. `curl https://sh.rustup.rs -sSf | sh; source $HOME/.cargo/env`)
  - if rust is already installed, you can simply update version with `rustup update`
- clang (clanglib or clang-devel or libclang-dev)
- ncurses and libs (ncurses, ncursesw5)
- zlib libs (zlib1g-dev or zlib-devel)
- pkc-config
- libssl-dev
- linux-headers (reported needed on Alpine linux)
- pkg-config (reported on Ubuntu)
- libssl-dev (reported on various builds) 

## Build steps

```sh
git clone https://github.com/mimblewimble/grin.git
cd grin
cargo build --release
```
Grin can also be built in debug mode (without the `--release` flag, but using the `--debug` or the `--verbose` flag) but this will render fast sync prohibitively slow due to the large overhead of cryptographic operations.

## Mining in Grin

Please note that all mining functions for Grin have moved into a separate, standalone package called
[grin_miner](https://github.com/mimblewimble/grin-miner). Once your Grin code node is up and running,
you can start mining by building and running grin-miner against your running Grin node.

### Build errors

See [Troubleshooting](https://github.com/mimblewimble/docs/wiki/Troubleshooting)

## What was built?

A successful build gets you:

 - `target/release/grin` - the main grin binary

Grin is still sensitive to the directory from which it's run. Make sure you
always run it within a directory that contains a `grin.toml` configuration and
stay consistent as to where it's run from.

With the included `grin.toml` unchanged, if you execute `cargo run` you get a
`.grin` subfolder that grin starts filling up with blockchain data.

While testing, put the grin binary on your path like this:

```
export PATH=/path/to/grin/dir/target/debug:$PATH
```
Where path/to/grin/dir is your absolute path to the root directory of your Grin installation. 

You can then run `grin` directly (try `grin help` for more options).

# Configuration

Grin attempts to run with sensible defaults, and can be further configured via
the `grin.toml` file. You should always ensure that this file is available to grin.
The supplied `grin.toml` contains inline documentation on all configuration
options, and should be the first point of reference for all options.

The `grin.toml` file can placed in one of several locations, using the first one it finds:

1. The current working directory
2. In the directory that holds the grin executable
3. {USER_HOME}/.grin

While it's recommended that you perform all grin server configuration via
`grin.toml`, it's also possible to supply command line switches to grin that
override any settings in the `grin.toml` file.

For help on grin commands and their switches, try:

```
grin help
grin wallet help
grin client help
```

# Using grin

The wiki page [How to use grin](https://github.com/mimblewimble/docs/wiki/How-to-use-grin)
and linked pages have more information on what features we have,
troubleshooting, etc.

## Docker

        # Build using all available cores
        docker build -t grin .

        # run in foreground
        docker run -it -v grin:/usr/src/grin grin

        # or in background
        docker run -it -d -v grin:/usr/src/grin grin

If you decide to use a persistent storage (e.g. ```-v grin:/usr/src/grin```) you will need grin.toml configuration file in it.

### Cross-platform builds

Rust (cargo) can build grin for many platforms, so in theory running `grin`
as a validating node on your low powered device might be possible.
To cross-compile `grin` on a x86 Linux platform and produce ARM binaries,
say, for a Raspberry Pi.

