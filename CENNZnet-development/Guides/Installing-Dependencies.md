# Installing dependencies

## Rust

The recommended way to install Rust is to follow the official instructions [here](https://www.rust-lang.org/tools/install).

## Docker

The easiest way to get started with Docker is via [Docker Desktop](https://www.docker.com/products/docker-desktop).

Alternatively, your favorite package manager can also be used like the below example:

```bash
$ choco install docker-desktop  # Windows
$ brew cask install docker      # macOS
$ sudo apt install docker.io    # Ubuntu
```

Note that if you want to build Cennznet locally(instead of running a docker image) on Windows, you may need to use the Linux subsystem. You can follow the instructions here to [Setting up Linux subsystem on Windows](CENNZnet-development/Guides/Set-up-Linux-Sub-system-for-Windows).