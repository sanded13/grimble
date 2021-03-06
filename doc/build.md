# Grimble - Build, Configuration, and Running

*Read this in other languages: [Español](build_ES.md).*

## Supported Platforms

Longer term, most platforms will likely be supported to some extent.
Grimble's programming language `rust` has build targets for most platforms.

What's working so far?

* Linux x86\_64 and MacOS [grimble + mining + development]
* Not Windows 10 yet [grimble kind-of builds. No mining yet. Help wanted!]

## Requirements

* rust 1.31+ (use [rustup]((https://www.rustup.rs/))- i.e. `curl https://sh.rustup.rs -sSf | sh; source $HOME/.cargo/env`)
  * if rust is already installed, you can simply update version with `rustup update`
* clang
* ncurses and libs (ncurses, ncursesw5)
* zlib libs (zlib1g-dev or zlib-devel)
* pkg-config
* libssl-dev
* linux-headers (reported needed on Alpine linux)
* llvm

For Debian-based distributions (Debian, Ubuntu, Mint, etc), all in one line (except Rust):

```sh
apt install build-essential cmake git libgit2-dev clang libncurses5-dev libncursesw5-dev zlib1g-dev pkg-config libssl-dev llvm
```

For Mac:

```sh
xcode-select --install
brew install --with-toolchain llvm
brew install pkg-config
brew install openssl
```

## Build steps

```sh
git clone https://github.com/grimblecurrency/grimble.git
cd grimble
cargo build --release
```

grimble can also be built in debug mode (without the `--release` flag, but using the `--debug` or the `--verbose` flag) but this will render fast sync prohibitively slow due to the large overhead of cryptographic operations.

## Build errors

See [Troubleshooting](https://github.com/mimblewimble/docs/wiki/Troubleshooting)

## What was built?

A successful build gets you:

* `target/release/grimble` - the main grimble binary

All data, configuration and log files created and used by grimble are located in the hidden
`~/.grimble` directory (under your user home directory) by default. You can modify all configuration
values by editing the file `~/.grimble/grimble-server.toml`.

It is also possible to have grimble create its data files in the current directory. To do this, run

```sh
grimble server config
```

Which will generate a `grimble-server.toml` file in the current directory, pre-configured to use
the current directory for all of its data. Running grimble from a directory that contains a
`grimble-server.toml` file will use the values in that file instead of the default
`~/.grimble/grimble-server.toml`.

While testing, put the grimble binary on your path like this:

```sh
export PATH=/path/to/grimble/dir/target/debug:$PATH
```

Where path/to/grimble/dir is your absolute path to the root directory of your grimble installation.

You can then run `grimble` directly (try `grimble help` for more options).

## Configuration

Grimble attempts to run with sensible defaults, and can be further configured via
the `grimble-server.toml` file. This file is generated by grimble on its first run, and
contains documentation on each available option.

While it's recommended that you perform all grimble server configuration via
`grimble-server.toml`, it's also possible to supply command line switches to grimble that
override any settings in the file.

For help on grimble commands and their switches, try:

```sh
grimble help
grimble wallet help
grimble client help
```

## Docker

```sh
docker build -t grimble -f etc/Dockerfile .
```
For floonet, use `etc/Dockerfile.floonet` instead

You can bind-mount your grimble cache to run inside the container.

```sh
docker run -it -d -v $HOME/.grimble:/root/.grimble grimble
```
If you prefer to use a docker named volume, you can pass `-v dotgrimble:/root/.grimble` instead.
Using a named volume copies default configurations upon volume creation

## Cross-platform builds

Rust (cargo) can build grimble for many platforms, so in theory running `grimble`
as a validating node on your low powered device might be possible.
To cross-compile `grimble` on a x86 Linux platform and produce ARM binaries,
say, for a Raspberry Pi.

## Using Grimble

The wiki page [Wallet User Guide](https://github.com/mimblewimble/docs/wiki/Wallet-User-Guide)
and linked pages have more information on what features we have,
troubleshooting, etc.

## Mining in Grimble

Grimble does not have separate mining software. All miners that can mine Grin can also mine 
Grimble. For example, [grin-miner](https://github.com/mimblewimble/grin-miner). 

For any miner to be able to communicate with your Grimble node, make sure that you have `enable_stratum_server = true`
in your `grimble-server.toml` configuration file and you have a wallet listener running (`grimble wallet listen`). 
