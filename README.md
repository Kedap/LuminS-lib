<h1 align="center">Lib for LuminS</h1>
<h3 align="center">Luminous Synchronize</h3>
<h4 align="center">A fast and reliable alternative to rsync for synchronizing local files</h4>

<p align="center">
  <a href="https://travis-ci.org/wchang22/LuminS"><img src="https://travis-ci.org/wchang22/LuminS.svg?branch=master" alt="Build Status" /></a>
  <a href="https://codecov.io/gh/wchang22/LuminS"><img src="https://codecov.io/gh/wchang22/LuminS/branch/master/graph/badge.svg" alt="Code Coverage" /></a>
  <a href="https://crates.io/crates/lms"><img src="http://meritbadge.herokuapp.com/lms" alt="Crate" /></a>
  <a href="https://docs.rs/lms"><img src="https://docs.rs/lms/badge.svg" alt="Docs" /></a>
</p>
<p align="center">
  <img src="examples/lumins.gif" alt="Demo"    
</p>

## Features

<table>
    <tr><td><b>100% Rust</b></td></tr>
    <tr><td><b>Powered by the <a href="https://github.com/rayon-rs/rayon">Rayon</a> library for high parallel perfomance</b></td></tr>
    <tr><td><b>Supported on Unix-based platforms</b></td></tr>
    <tr><td><b>Extremely fast at synchronizing directories with large quantities of files</b></td></tr>
    <tr><td><b>Multithreaded copy, remove, and sync</b></td></tr>
    <tr><td><b>A progress bar using <a href="https://github.com/mitsuhiko/indicatif">indicatif</a></b></td></tr>
</table>


## Copy

```rust
use lms_lib::{core, parse::Flag};
use std::path::Path;

main () {
	let src = Path::new("/foo/bar/hello.txt");
	let dest = Path::new("/bar/foo/hi.txt");
	core::copy(&src, &dest, Flag::empty())?;
}
```
#### Sync

```rust
use lms_lib::{core, parse::Flag};
use std::path::Path;

main () {
	let src = Path::new("/foo/bar/hello.txt");
	let dest = Path::new("/bar/foo/hi.txt");
	core::synchronize(&src, &dest, Flag::empty())?;
}
```

#### Remove

```rust
use lms_lib::{core, parse::Flag};
use std::path::Path;

main () {
	let target = Path::new("/bar/foo/hi.txt");
	core::remove(&target, Flag::empty())?;
}
```

## Benchmarks

Using [hyperfine](https://github.com/sharkdp/hyperfine) on an Intel i7-8550U with the following 2 test folders,

| Directory | Directory Size | Number of Files |
| --------- | -------------- | --------------- |
| 1         | 88MB           | 7262            |
| 2         | 105MB          | 252             |

| Command                | Directory       | Time                          |
| ---------------------- | --------------- | ----------------------------- |
| **lms sync**           | 1               | **179.1 ms** ± 5.1 ms         |
| rsync -r --delete      | 1               | 717.8 ms ± 41.1 ms            |
| **lms cp**             | 1               | **117.3 ms** ± 3.6 ms         |
| cp -r                  | 1               | 283.4 ms ± 13.2 ms            |
| **lms rm**             | 1               | **147.6 ms** ± 8.6 ms         |
| rm -rf                 | 1               | 180.7 ms ± 4.3 ms             |
| ---------------------- | --------------- | ----------------------------- |
| **lms sync**           | 2               | **101.2 ms** ± 24.8 ms        |
| rsync -r --delete      | 2               | 442.2 ms ± 19.6 ms            |
| **lms cp**             | 2               | **33.8 ms** ± 2.8 ms          |
| cp -r                  | 2               | 143.5 ms ± 18.8 ms            |
| **lms rm**             | 2               | **10.0 ms** ± 2.8 ms          |
| rm -rf                 | 2               | 27.4 ms ± 0.8 ms              |

Of course, these benchmarks can be highly dependent on CPU and IO devices.

## Contributions

Suggestions, issues, and pull requests are welcome!
