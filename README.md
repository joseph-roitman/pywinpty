# PyWinpty: Pseudoterminals for Windows in Python

[![Project License - MIT](https://img.shields.io/pypi/l/pywinpty.svg)](./LICENSE.txt)
[![pypi version](https://img.shields.io/pypi/v/pywinpty.svg)](https://pypi.org/project/pywinpty/)
[![conda version](https://img.shields.io/conda/vn/conda-forge/pywinpty.svg)](https://www.anaconda.com/download/)
[![download count](https://img.shields.io/conda/dn/conda-forge/pywinpty.svg)](https://www.anaconda.com/download/)
[![Downloads](https://pepy.tech/badge/pywinpty)](https://pepy.tech/project/pywinpty)
[![OpenCollective Backers](https://opencollective.com/spyder/backers/badge.svg?color=blue)](#backers)
[![Join the chat at https://gitter.im/spyder-ide/public](https://badges.gitter.im/spyder-ide/spyder.svg)](https://gitter.im/spyder-ide/public)<br>
[![PyPI status](https://img.shields.io/pypi/status/pywinpty.svg)](https://github.com/spyder-ide/pywinpty)
[![Windows tests](https://github.com/spyder-ide/pywinpty/actions/workflows/windows_build.yml/badge.svg)](https://github.com/spyder-ide/pywinpty/actions/workflows/windows_build.yml)

*Copyright © 2017– Spyder Project Contributors*


## Overview

PyWinpty allows creating and communicating with Windows processes that receive input and print outputs via console input and output pipes. PyWinpty supports both the native [ConPTY](https://devblogs.microsoft.com/commandline/windows-command-line-introducing-the-windows-pseudo-console-conpty/) interface and the previous, fallback [winpty](https://github.com/rprichard/winpty) library.


## Dependencies
To compile pywinpty sources, you must have [Rust](https://rustup.rs/) and MSVC installed.
Optionally, you can also have Winpty's C header and library files available on your include path.


## Installation
You can install this library by using conda or pip package managers, as it follows:

Using conda (Recommended):
```bash
conda install pywinpty
```

Using pip:
```bash
pip install pywinpty
```

## Building from source

To build from sources, you will require both a working stable or nightly Rust toolchain with
target `x86_64-pc-windows-msvc`, which can be installed using [rustup](https://rustup.rs/).
Additionally, you will require a working installation of [Microsoft Visual Studio C/C++](https://visualstudio.microsoft.com/es/vs/features/cplusplus/) compiler.

Optionally, this library can be linked against winpty library, which you can install using conda-forge:

```batch
conda install winpty -c conda-forge
```

If you don't want to use conda, you will need to have the winpty binaries and headers available on your PATH.

Finally, pywinpty uses [Maturin](https://github.com/PyO3/maturin) as the build backend, which can be installed using `pip`:

```batch
pip install maturin
```

To test your compilation environment settings, you can build pywinpty sources locally, by
executing:

```bash
maturin develop
```

This package depends on the following Rust crates:

* [PyO3](https://github.com/PyO3/pyo3): Library used to produce Python bindings from Rust code.
* [CXX](https://github.com/dtolnay/cxx): Call C++ libraries from Rust.
* [Maturin](https://github.com/PyO3/maturin): Build system to build and publish Rust-based Python packages.
* [Windows](https://github.com/microsoft/windows-rs): Rust for Windows.

## Package usage
Pywinpty offers a single python wrapper around winpty library functions.
This implies that using a single object (``winpty.PTY``) it is possible to access to all functionality, as it follows:

```python
# High level usage using `spawn`
from winpty import PtyProcess

proc = PtyProcess.spawn('python')
proc.write('print("hello, world!")\r\n')
proc.write('exit()\r\n')
while proc.isalive():
	print(proc.readline())

# Low level usage using the raw `PTY` object
from winpty import PTY

# Start a new winpty-agent process of size (cols, rows)
cols, rows = 80, 25
process = PTY(cols, rows)

# Spawn a new console process, e.g., CMD
process.spawn(br'C:\windows\system32\cmd.exe')

# Read console output (Unicode)
process.read()

# Write input to console (Unicode)
process.write(b'Text')

# Resize console size
new_cols, new_rows = 90, 30
process.set_size(new_cols, new_rows)

# Know if the process is alive
alive = process.isalive()

# End winpty-agent process
del process
```

## Running tests
We use pytest to run tests as it follows (after calling ``maturin develop``), the test suite depends
on pytest-lazy-fixture, which can be installed via pip:

```batch
pip install pytest pytest-lazy-fixture flaky
```

All the tests can be exceuted using the following command

```bash
python runtests.py
```


## Changelog
Visit our [CHANGELOG](CHANGELOG.md) file to learn more about our new features and improvements.


## Contribution guidelines
We follow PEP8 and PEP257 for pure python packages and Rust to compile extensions. We use MyPy type annotations for all functions and classes declared on this package. Feel free to send a PR or create an issue if you have any problem/question.


## Backers

Support us with a monthly donation and help us continue our activities.

[![Backers](https://opencollective.com/spyder/backers.svg)](https://opencollective.com/spyder#support)


## Sponsors

Become a sponsor to get your logo on our README on Github.

[![Sponsors](https://opencollective.com/spyder/sponsors.svg)](https://opencollective.com/spyder#support)
