# MOTHER 3 Decompilation

A work-in-progress decompilation of **MOTHER 3** for the Game Boy Advance.

This repository is intended to organize matching source code, game data/assets, build tooling, and documentation while preserving a clean separation between reconstructed code and original game material.

## Status

Early repository setup. The active decompilation work will be imported here once the repository structure and build notes are in place.

## Repository layout

- `src/` — reconstructed C/C++ source
- `include/` — headers
- `asm/` — assembly and non-matching functions where still needed
- `data/` — reconstructed/generated game data
- `assets/` — extracted or generated asset descriptions; copyrighted ROM content should not be committed
- `tools/` — helper scripts and decompilation tooling
- `docs/` — build, matching, and contributor notes

## Building

Build instructions will be documented in `docs/BUILDING.md` when the current decompilation tree is imported.

A legally obtained MOTHER 3 ROM may be required by the build process. Do not commit ROM images or other copyrighted game dumps to this repository.

## Goals

- Reconstruct the game into maintainable source form.
- Improve function and data matching against the original GBA release.
- Keep platform-specific dependencies isolated enough to support future native ports.
- Avoid decompiling or reimplementing standard runtime/library code when an equivalent standard implementation can be linked instead.

## Credits

This project builds on prior community decompilation work, including the existing MOTHER 3 decompilation efforts that provided the starting point for this project.
AI (ChatGPT Codex) heavily used for decompilation (but it byte-matches so it's fine)

## Disclaimer

This is an unofficial fan reverse-engineering project. MOTHER 3 and related properties belong to their respective rights holders.
