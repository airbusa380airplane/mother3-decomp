# Building

## Current status

This repository is a scaffold: the source/tool directories contain placeholders,
and there is no Makefile, compiler, extraction script, or ROM checksum file yet.
It cannot currently build a ROM.

The commands below describe the **upstream reference workflow**, checked against
[Kurausukun/mother3 at `5637f2c`](https://github.com/Kurausukun/mother3/tree/5637f2c04fe761f9a3e05335cad5103c4af1da23).
Run them in a separate upstream checkout, not this scaffold. **Project assumption:**
the eventual import will retain that workflow; its paths, toolchain pins, and
matching result still need confirmation. These instructions have been checked
against the build files, but no ROM build was run for this documentation change.

## Prerequisites and installation

- A legally obtained, local, unmodified Japanese MOTHER 3 ROM (revision 0).
- Git, Bash, curl, tar, and `sha1sum` (or `shasum -a 1`).
- Docker with a running daemon accessible to your user and support for
  `linux/amd64` containers. On another CPU architecture this requires emulation.
  Windows users need a Bash environment with Docker integration, such as WSL;
  that host configuration has not been validated here.
- The separate `notyourav/agbcc` **`cp`** compiler bundle below. Installing
  devkitARM alone does not supply upstream's `agbcc`/`agbcp` compilers.

Example host-package installation for Debian/Ubuntu (package availability and
versions depend on the distribution; this is not a tested project version pin):

```sh
sudo apt update
sudo apt install git curl ca-certificates tar coreutils docker.io
docker info
```

Configure/start Docker for your host if `docker info` fails before continuing.
Then, from a directory **outside this repository**:

```sh
git clone https://github.com/Kurausukun/mother3.git mother3-upstream
cd mother3-upstream
git checkout 5637f2c04fe761f9a3e05335cad5103c4af1da23
mkdir -p tools/agbcc
curl -fL https://github.com/notyourav/agbcc/releases/download/cp/agbcc.tar.gz -o /tmp/mother3-agbcc.tar.gz
tar -xzf /tmp/mother3-agbcc.tar.gz -C tools/agbcc
docker pull --platform=linux/amd64 devkitpro/devkitarm:latest
```

Upstream's scripts use `devkitpro/devkitarm:latest`, set
`DEVKITPRO=/opt/devkitpro` and `DEVKITARM=/opt/devkitpro/devkitARM`, and mount
the checkout at `/work`. The build needs GNU Make, Python 3, devkitARM's ARM
preprocessor/binutils, and host C/C++ tools; Salsa specifically requires
**CMake >= 3.13** and **C++17**. Neither the Docker image digest nor an exact
devkitARM/compiler-bundle release version is pinned by the inspected workflow.
Record the actual image digest, compiler source/release, and tool versions when
reporting a build; do not assume `latest` is reproducible. Upstream also documents
building `agbcc` from its `cp` branch, with an Apple Silicon limitation.

## Verify the local ROM

In the upstream checkout, copy your own ROM to `baserom.gba` at the repository
root, then verify it **before extraction**:

```sh
cp /path/to/your/legally-obtained-mother3.gba baserom.gba
printf '%s  %s\n' 4f0f493e12c2a8c61b2d809af03f7abf87a85776 baserom.gba | sha1sum -c -
```

Expect `baserom.gba: OK`. If using macOS's hash utility, replace
`sha1sum -c -` with `shasum -a 1 -c -`. Stop on a mismatch: patched/translated
ROMs are not the upstream matching target. This hash comes from upstream's
README and `mother3.sha1`; it is **not yet independently confirmed for this
project's pending import**. The scaffold ignores `baserom/`, but upstream
expects the root file `baserom.gba`.

## Set up, build, and compare

Run from the prepared upstream checkout:

```sh
bash setup.sh
bash build.sh all
sha1sum -c mother3.sha1
```

`setup.sh` runs `make setup`: it builds helper tools and extracts
`assets/mainscript.salsa`, `assets/misctext.salsa`, and `assets/logic.salsa`
from your ROM. It does **not** install the separate compiler bundle.
`build.sh all` runs `make -s all` inside Docker. Upstream defaults to
`GAME_REGION=JAPAN`, `GAME_REVISION=0`, `DEBUG=0`, and `COMPARE=1`;
the normal build already checks the output hash.

Expected outputs are `mother3.gba`, `mother3.elf`, and `mother3.map` at the
root, intermediates under `build/mother3/`, and `cur_progress.txt`.
Successful verification reports `mother3.gba: OK`. A completed compile or
progress percentage alone is not proof of a matching ROM. Investigate input,
toolchain, or source differences on failure; disabling comparison does not
establish a match.

## Public files versus local files

**Commit:** reconstructed source/headers, permitted assembly/data definitions,
helper-tool source, build recipes, checksums, and documentation. Preserve
attribution and license notices on imports.

**Keep local:** ROM images (input and output), extracted copyrighted assets,
proprietary dumps, installed compiler binaries/libraries, and generated build
files. Never attach them to issues, pull requests, releases, or CI artifacts.

This scaffold ignores `/baserom/`, `/build/`, `*.gba`, `*.o`, and `*.elf`,
but does **not** comprehensively ignore upstream's extracted `assets/`,
`tools/agbcc/`, root `*.map`, or `cur_progress.txt`. Before importing or
running that workflow here, add precise ignore rules or adapt generation paths
to the project's `build/` convention. A text-format asset dump is still
extracted content. Review `git status --short` and `git diff --cached` before
every commit; stage only intended files and never force-add local material.

### Upstream references

- [Installation guide](https://github.com/Kurausukun/mother3/blob/5637f2c04fe761f9a3e05335cad5103c4af1da23/INSTALL.md)
- [Makefile](https://github.com/Kurausukun/mother3/blob/5637f2c04fe761f9a3e05335cad5103c4af1da23/Makefile) and [configuration](https://github.com/Kurausukun/mother3/blob/5637f2c04fe761f9a3e05335cad5103c4af1da23/config.mk)
- [Setup wrapper](https://github.com/Kurausukun/mother3/blob/5637f2c04fe761f9a3e05335cad5103c4af1da23/setup.sh) and [build wrapper](https://github.com/Kurausukun/mother3/blob/5637f2c04fe761f9a3e05335cad5103c4af1da23/build.sh)
- [ROM checksum](https://github.com/Kurausukun/mother3/blob/5637f2c04fe761f9a3e05335cad5103c4af1da23/mother3.sha1) and [Salsa requirements](https://github.com/Kurausukun/mother3/blob/5637f2c04fe761f9a3e05335cad5103c4af1da23/tools/salsa/CMakeLists.txt)
