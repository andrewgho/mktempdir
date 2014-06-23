mktempdir.sh - securely create temporary directory
==================================================

This repository provides a `mktempdir()` shell function which securely
and portably creates a temporary directory. It should work on most
modern Unix and Unix-like systems, including FreeBSD, OpenBSD, Linux,
OS X, Cygwin, and MinGW environments.

Synopsis
--------

Create a temporary directory, `chdir` to it, and touch a file there:

```sh
tempdir=`mktempdir` && (cd "$tempdir" && touch tempfile)
```

Pass a prefix for the directory (the default is to use the basenae of
the invoking program):

```sh
tempdir=`mktempdir myprefix` && (cd "$tempdir" && touch tempfile)
```

Description
-----------

Many shell scripts create temporary directories by using some
combination of program name and PID:

```sh
tempdir=/var/tmp/`basename "$0"`."$$"
rm -rf "$tempdir"
mkdir "$tempdir"
```

This has several disadvantages, most notably, PIDs can be reused in most
systems, and PIDs can also be guessed, raising security concerns for
scripts with elevated privileges who create temporary directories this
way. The `mktemp` command was created (originally in OpenBSD) to provide
a more general and secure way to create temporary files and directories.

This script provides an interface `mktempdir()` which runs `mktemp` in a
manner portable across most environments where shell scripts usually
run, using only traditional Bourne shell syntax (no `bash` specific
syntax), requiring only generally available `mktemp`, `printf`, and
`sed` programs, and passing only widely supported options to those
commands.

Of the prerequisites, `mktemp` is the least generally available, and has
the least consistent semantics across platforms. That being said, it is
available on FreeBSD (from version 2.2.7), OpenBSD (from version 2.1),
and most Linux distributions (usually in a `mktemp` package included in
even base or minimal installations). It is also present by default in OS
X, and available as a package in Cygwin and MinGW installations.

Most `mktemp` variants should accept the `-dqt` options passed by
`mktempdir()`, and special care is taken to handle templates with
prefixed dashes (at least some older FreeBSD `mktemp` variants treate a
template of, for example, `-sh` as an option, die on an unknown option
error, and do not use the convention of taking `--` as signifying the
end of command line options).

License
-------

```
Copyright (c) 2014, Andrew Ho.
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions
are met:

Redistributions of source code must retain the above copyright notice,
this list of conditions and the following disclaimer.

Redistributions in binary form must reproduce the above copyright
notice, this list of conditions and the following disclaimer in the
documentation and/or other materials provided with the distribution.

Neither the name of the author nor the names of its contributors may
be used to endorse or promote products derived from this software
without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
"AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
```

Meta
----

* Home: <https://github.com/andrewgho/mktempdir>
* Bugs: <https://github.com/andrewgho/mktempdir/issues>
* Author: <https://github.com/andrewgho>

### Prior Art ###

The following links include discussions and similar implementations for
solving the problem of portably creating temporary directories:

* http://content.hccfl.edu/pollock/ShScript/TempFile.htm
* http://unix.stackexchange.com/questions/30091/fix-or-alternative-for-mktemp-in-os-x
