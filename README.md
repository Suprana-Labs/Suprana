# PLEASE READ: This repo is now a public archive

This repo still exists in archived form, feel free to fork any reference
implementations it still contains.

See Agave, the Suprana validator implementation from Anza: https://github.com/anza-xyz/agave

---

<p align="center">
  <a href="https://Suprana.net">
    <img alt="Suprana" src="https://i.imgur.com/IKyzQ6T.png" width="250" />
  </a>
</p>

[![Suprana crate](https://img.shields.io/crates/v/suprana-core.svg)](https://crates.io/crates/suprana-core)
[![Suprana documentation](https://docs.rs/suprana-core/badge.svg)](https://docs.rs/suprana-core)
[![Build status](https://badge.buildkite.com/8cc350de251d61483db98bdfc895b9ea0ac8ffa4a32ee850ed.svg?branch=master)](https://buildkite.com/suprana-labs/suprana/builds?branch=master)
[![codecov](https://codecov.io/gh/Suprana-Labs/Suprana/branch/master/graph/badge.svg)](https://codecov.io/gh/suprana-labs/suprana)

# Building

## **1. Install rustc, cargo and rustfmt.**

```bash
$ curl https://sh.rustup.rs -sSf | sh
$ source $HOME/.cargo/env
$ rustup component add rustfmt
```

When building the master branch, please make sure you are using the latest stable rust version by running:

```bash
$ rustup update
```

When building a specific release branch, you should check the rust version in `ci/rust-version.sh` and if necessary, install that version by running:

```bash
$ rustup install VERSION
```

Note that if this is not the latest rust version on your machine, cargo commands may require an [override](https://rust-lang.github.io/rustup/overrides.html) in order to use the correct version.

On Linux systems you may need to install libssl-dev, pkg-config, zlib1g-dev, protobuf etc.

On Ubuntu:

```bash
$ sudo apt-get update
$ sudo apt-get install libssl-dev libudev-dev pkg-config zlib1g-dev llvm clang cmake make libprotobuf-dev protobuf-compiler
```

On Fedora:

```bash
$ sudo dnf install openssl-devel systemd-devel pkg-config zlib-devel llvm clang cmake make protobuf-devel protobuf-compiler perl-core
```

## **2. Download the source code.**

```bash
$ git clone https://github.com/suprana-labs/suprana.git
$ cd suprana
```

## **3. Build.**

```bash
$ ./cargo build
```

# Testing

**Run the test suite:**

```bash
$ ./cargo test
```

### Starting a local testnet

Start your own testnet locally, instructions are in the [online docs](https://docs.supranalabs.com/clusters/benchmark).

### Accessing the remote development cluster

- `devnet` - stable public cluster for development accessible via
  devnet.suprana.net. Runs 24/7. Learn more about the [public clusters](https://docs.supranalabs.com/clusters)

# Benchmarking

First, install the nightly build of rustc. `cargo bench` requires the use of the
unstable features only available in the nightly build.

```bash
$ rustup install nightly
```

Run the benchmarks:

```bash
$ cargo +nightly bench
```

# Release Process

The release process for this project is described [here](RELEASE.md).

# Code coverage

To generate code coverage statistics:

```bash
$ scripts/coverage.sh
$ open target/cov/lcov-local/index.html
```

Why coverage? While most see coverage as a code quality metric, we see it primarily as a developer
productivity metric. When a developer makes a change to the codebase, presumably it's a _solution_ to
some problem. Our unit-test suite is how we encode the set of _problems_ the codebase solves. Running
the test suite should indicate that your change didn't _infringe_ on anyone else's solutions. Adding a
test _protects_ your solution from future changes. Say you don't understand why a line of code exists,
try deleting it and running the unit-tests. The nearest test failure should tell you what problem
was solved by that code. If no test fails, go ahead and submit a Pull Request that asks, "what
problem is solved by this code?" On the other hand, if a test does fail and you can think of a
better way to solve the same problem, a Pull Request with your solution would most certainly be
welcome! Likewise, if rewriting a test can better communicate what code it's protecting, please
send us that patch!

# Disclaimer

All claims, content, designs, algorithms, estimates, roadmaps,
specifications, and performance measurements described in this project
are done with the Suprana Labs, Inc. (“SL”) good faith efforts. It is up to
the reader to check and validate their accuracy and truthfulness.
Furthermore, nothing in this project constitutes a solicitation for
investment.

Any content produced by SL or developer resources that SL provides are
for educational and inspirational purposes only. SL does not encourage,
induce or sanction the deployment, integration or use of any such
applications (including the code comprising the Suprana blockchain
protocol) in violation of applicable laws or regulations and hereby
prohibits any such deployment, integration or use. This includes the use of
any such applications by the reader (a) in violation of export control
or sanctions laws of the United States or any other applicable
jurisdiction, (b) if the reader is located in or ordinarily resident in
a country or territory subject to comprehensive sanctions administered
by the U.S. Office of Foreign Assets Control (OFAC), or (c) if the
reader is or is working on behalf of a Specially Designated National
(SDN) or a person subject to similar blocking or denied party
prohibitions.

The reader should be aware that U.S. export control and sanctions laws prohibit
U.S. persons (and other persons that are subject to such laws) from transacting
with persons in certain countries and territories or that are on the SDN list.
Accordingly, there is a risk to individuals that other persons using any of the
code contained in this repo, or a derivation thereof, may be sanctioned persons
and that transactions with such persons would be a violation of U.S. export
controls and sanctions law.
