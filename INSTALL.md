# Acquiring the Cryptol course material

All of the Cryptol course material (presentations, labs, supporting
data files) is available on [GitHub](https://github.com).  You can
clone or download the files using the `clone` button on the GitHub
page, or you can use the command line to acquire a copy by ensuring
you're in a writable working directory and issuing `git clone
https://github.com/weaversa/cryptol-course.git` *(no password or keys
required)*, or if you don't have `git` installed, `curl -L
-ocryptol-course.zip
https://github.com/weaversa/cryptol-course/archive/master.zip && unzip
cryptol-course.zip`.

The presentation material is formatted in Markdown (.md) files.  These
can be viewed directly in most browsers by installing a Markdown
viewer extension *(an exercise left to the reader)*, or by accessing the
material directly from the GitHub website.

# Installing Cryptol and SAW

This course currently focuses on learning to program in Cryptol. As
such, you will greatly benefit from installing the Cryptol
interpreter. Supported platforms include CentOS, Ubuntu, MacOS, and
Windows 10.

Some challenge labs make use of SAW, a companion verification tool
associated with Cryptol. However, SAW is not a requirement for success
here.

## Option 1: Docker (*Preferred Installation Method*)

[Docker](https://www.docker.com) containers are available for both
[Cryptol](https://hub.docker.com/repository/docker/cryptolcourse/cryptol)
and
[SAW](https://hub.docker.com/repository/docker/cryptolcourse/saw). To
use these Docker containers you'll *first* need to install `docker` on
your computer. Instructions for installing `docker` on Linux, MacOS,
and Windows 10 can be found at
[https://docs.docker.com/get-docker](https://docs.docker.com/get-docker). Once
Docker has been [installed](https://docs.docker.com/get-docker), it is
easy to `pull` and `run` these containers. *(Note that this docker
approach may require `sudo` privileges. If so, and you don't have such
privileges, follow the steps in [Option 2](#option-2-homebrew) or
[Option 3](#option-3-downloading-pre-built-cryptol-and-saw-binaries)
for user-mode solutions.)*

```shell
$ docker pull cryptolcourse/cryptol
...
$ docker run --rm -it cryptolcourse/cryptol
┏━╸┏━┓╻ ╻┏━┓╺┳╸┏━┓╻
┃  ┣┳┛┗┳┛┣━┛ ┃ ┃ ┃┃
┗━╸╹┗╸ ╹ ╹   ╹ ┗━┛┗━╸
version 2.8.0

Loading module Cryptol
Cryptol> :sat \(x:[4]) -> (x + 1 < x)
(\(x : [4]) -> (x + 1 < x)) 0xf = True
(Total Elapsed Time: 0.006s, using Z3)
Cryptol> :quit
```

```shell
$ docker pull cryptolcourse/saw
...
$ docker run --rm -it cryptolcourse/saw
 ┏━━━┓━━━┓━┓━┓━┓
 ┃ ━━┓ ╻ ┃ ┃ ┃ ┃
 ┣━━ ┃ ╻ ┃┓ ╻ ┏┛
 ┗━━━┛━┛━┛┗━┛━┛ version 0.5 (<non-dev-build>)
sawscript> :help sat
Description
-----------

    sat : ProofScript SatResult -> Term -> TopLevel SatResult

Use the given proof script to attempt to prove that a term is
satisfiable (true for any input). Returns a proof result that can
be analyzed with 'caseSatResult' to determine whether it represents
a satisfying assignment or an indication of unsatisfiability.
sawscript> :quit
```

Details:
- Instructions for installing `docker` on your system (Linux, MacOS, and
Windows 10) can be found at
[https://docs.docker.com/get-docker](https://docs.docker.com/get-docker).
- `docker run --rm -it` indicates that the commands are to be run in an interactive
TTY, and the generated container will be removed upon exit.
- If you are currently in the root of this repository, you can use
`-v` and `--env` to mount the repository in the docker container and set
the `CRYPTOLPATH` environment variable for access to this repository's
Cryptol modules. This environment variable is used by both Cryptol and
SAW. For example:

```shell
$ docker run --rm --read-only --mount type=bind,src=$(pwd),dst=/mnt/cryptol-course --env CRYPTOLPATH=/mnt/cryptol-course -it cryptolcourse/cryptol
┏━╸┏━┓╻ ╻┏━┓╺┳╸┏━┓╻
┃  ┣┳┛┗┳┛┣━┛ ┃ ┃ ┃┃
┗━╸╹┗╸ ╹ ╹   ╹ ┗━┛┗━╸
version 2.8.0

Loading module Cryptol
Cryptol> :module specs::Misc::Sudoku
Loading module specs::Misc::Sudoku
specs::Misc::Sudoku> :quit
```

## Option 2: Homebrew

[Homebrew](https://brew.sh) is a package manager for MacOS, Linux, and
Windows Subsystem for Linux. Instructions for installing Homebrew can
be found on [Homebrew's website](https://brew.sh), and consist of
pasting a simple command into a shell prompt.

Once Homebrew is installed, Cryptol (along with its `z3` dependency)
can be installed via:

```shell
brew update && brew install cryptol
```

Unfortunately, SAW is not available via Homebrew.


## Option 3: Downloading pre-built Cryptol and SAW binaries

### Downloading Cryptol and SAW

Galois provides releases of Cryptol at
https://cryptol.net/downloads.html and releases of SAW at
https://saw.galois.com/downloads.html. For Linux variants, Cryptol
comes bundled with SAW, so you will only need to install SAW to get
both tools. *(Note that the Ubuntu files indicate Ubuntu14.04, but
they work on later versions of Ubuntu as well.)*

The `bin` directory (containing `cryptol` and/or `saw`) of the archive
you downloaded should be placed in your system path.

For CentOS, Ubuntu, or MacOS, the whole process would look something
like (depending on the which OS variant you have):

```shell
$ curl -fsSL https://github.com/GaloisInc/saw-script/releases/download/v0.5/saw-0.5-Ubuntu14.04-64.tar.gz | tar -xz
$ export PATH=$(pwd)/saw-0.5-Ubuntu14.04-64/bin:${PATH}
```

*If you are running Windows 10, or you _only_ want Cryptol, you can find
an installer at https://cryptol.net/downloads.html. Note that these
instructions do not currently provide any details on how to install
Cryptol on Windows 10, though the installer is self explanatory.*

*Galois also provides a server with nightly builds of SAW for CentOS,
Ubuntu, and MacOS, which you can find at
https://saw.galois.com/builds/nightly.*

### Downloading Z3

Both Cryptol and SAW require a tool called `z3`. This tool is not
bundled with Cryptol or SAW, so it must be installed separately.
*(Note that the version of `z3` available via default `apt` repos is old and
incompatible with this course.)*

Pre-built binaries for Z3 can be found at
https://github.com/Z3Prover/z3/releases.  Z3 can also be compiled from
source without much, if any, trouble. The source is available at
https://github.com/Z3Prover/z3.  The base directory (containing `z3`
and more) of the zip file you download or built should be placed in
your system path.

For CentOS, Ubuntu, or MacOS, the whole process would look something
like (depending on which OS build and version you download):

```shell
$ curl -fsSL https://github.com/Z3Prover/z3/releases/download/z3-4.8.8/z3-4.8.8-x64-osx-10.14.6.zip -o z3-4.8.8-x64-osx-10.14.6.zip
$ unzip -j z3-4.8.8-x64-osx-10.14.6.zip -d z3-4.8.8
$ export PATH=$(pwd)/z3-4.8.8:${PATH}
```

It may behoove you at this point to add these paths to your user profile
by adding an `export PATH=...` line to your `.bashrc`, `.profile`,
etc. file to ensure future access to the tools.


## Checking the Installation

To verify that Cryptol and SAW work, simply run their interpreters and
see that they report no errors.

```shell
$ cryptol
┏━╸┏━┓╻ ╻┏━┓╺┳╸┏━┓╻
┃  ┣┳┛┗┳┛┣━┛ ┃ ┃ ┃┃
┗━╸╹┗╸ ╹ ╹   ╹ ┗━┛┗━╸
version 2.8.1 (e914cef)
https://cryptol.net  :? for help

Loading module Cryptol
Cryptol> :quit
```

```shell
$ saw
 ┏━━━┓━━━┓━┓━┓━┓
 ┃ ━━┓ ╻ ┃ ┃ ┃ ┃
 ┣━━ ┃ ╻ ┃┓ ╻ ┏┛
 ┗━━━┛━┛━┛┗━┛━┛ version 0.4.0.99 (eeef9a13)

sawscript> :quit
```

## Help

For more help installing Cryptol or SAW, please refer to the
documentation provided in their respective repositories, found here:

* Cryptol: https://github.com/GaloisInc/cryptol
* SAW: https://github.com/GaloisInc/saw-script

-----

# Running Cryptol

To load a literate document into Cryptol, change to your
`cryptol-course` directory in a terminal (Linux) or command prompt
(Windows 10), then run Cryptol via a locally installed binary or
Docker container. We'll use
[labs/Demos/OneTimePad.md](labs/Demos/OneTimePad.md) as an
example. The literate document can be provided as a parameter when
starting the Cryptol interpreter:

```shell
.../cryptol-course$ cryptol labs/Demos/OneTimePad.md
    ...
Loading module Cryptol
Loading module labs::Demos::OneTimePad
labs::Demos::OneTimePad>
```

Alternatively, you can use the `:module` or `:load` command from
within Cryptol to load the document. (Either ensure the cryptol
intepreter is started in the `cryptol-course` directory, or set the
environment variable CRYPTOLPATH to point to that directory.)

```shell
.../cryptol-course$ cryptol
    ...
Loading module Cryptol
Cryptol> :module labs::Demos::OneTimePad
Loading module labs::Demos::OneTimePad
labs::Demos::OneTimePad>
```

### Using Docker on Linux

```shell
.../cryptol-course> docker run --rm --read-only --mount type=bind,src=$(pwd),dst=/mnt/cryptol-course --env CRYPTOLPATH=/mnt/cryptol-course -it cryptolcourse/cryptol
    ...
Loading module Cryptol
Cryptol> :module labs::Demos::OneTimePad
Loading module labs::Demos::OneTimePad
labs::Demos::OneTimePad>
```

### Using Docker on Windows 10
```shell
...\cryptol-course> docker run --rm --read-only --mount type=bind,src=%CD%,dst=/mnt/cryptol-course --env CRYPTOLPATH=/mnt/cryptol-course -it cryptolcourse/cryptol
    ...
Loading module Cryptol
Cryptol> :module labs::Demos::OneTimePad
Loading module labs::Demos::OneTimePad
labs::Demos::OneTimePad>
```


## Editors & IDEs

Support exists for Cryptol (such as syntax highlighting and
interpreter bindings) in a number of popular software development
tools.


### Emacs

A Cryptol major mode for Emacs can be found here:
https://github.com/victoredwardocallaghan/cryptol.vim


### Vim

A Vim plugin for Cryptol can be found here:
https://github.com/victoredwardocallaghan/cryptol.vim


### VS Code

The [Cryptol
Highlighting](https://github.com/GaloisInc/cryptol-vscode.git) plugin
provides syntax highlighting and an interface to a local Cryptol
installation (e.g. evaluate the current expression or get its type).

The local `.vscode` configuration in the `cryptol-course` repo
supports running a `cryptolcourse/cryptol` Docker image via `Terminal
> Run Task... > cryptol-docker` in the VS Code menu bar.
