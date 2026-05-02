# CLAUDE.md

Guidance for Claude (and other AI assistants) working in this repository.

## Project overview

This repo is **our fork** of [`microsoft/MS-DOS`](https://github.com/microsoft/MS-DOS) — Microsoft's published historical source release of MS-DOS:

- **v1.25** and **v2.0** — original source and period binaries, originally shared via the Computer History Museum (March 2014).
- **v4.0** — source for the IBM/Microsoft jointly developed MS-DOS 4.00, including a working `nmake`-based build and the period MASM/LINK toolchain.
- **v4.0-ozzie** — beta-era documentation PDFs only (no source).

The code is overwhelmingly **8086 assembly (MASM)**. Because this is a fork, **edits and experiments are expected here** — that's the point.

## Upstream context (not a constraint on this fork)

The upstream `microsoft/MS-DOS` repo is intentionally static. From its `README.md`:

> The source files in this repo are for historical reference and will be kept static, so please **don't send** Pull Requests suggesting any modifications to the source files, but feel free to fork this repo and experiment.

That policy applies **upstream**, not here. In this fork:

- Modifying any file — including original `.ASM` sources — is fine when the user asks for it.
- Don't propose or open PRs against `microsoft/MS-DOS`. PRs against *this* fork's own remote are fine when requested.
- Treat `.EXE`, `.COM`, `.OVR`, and `.PDF` as opaque binary artifacts — don't try to text-edit them.

## Repository layout

| Path | What's there |
|---|---|
| `v1.25/source/` | ~6 core `.ASM` files: `MSDOS.ASM`, `COMMAND.ASM`, `IO.ASM`, `ASM.ASM`, `HEX2BIN.ASM`, `TRANS.ASM`, `STDDOS.ASM`. |
| `v1.25/bin/` | Period-built binaries matching the v1.25 sources. |
| `v2.0/source/` | ~50 `.ASM` files (`COMMAND`, `ALLOC`, `BUF`, `CHKDSK`, `COPY`, `DEBASM`, …) plus `.OVR` overlays. |
| `v2.0/bin/` | v2.0 binaries / overlays. |
| `v4.0/src/` | 580+ `.ASM` files organized into subdirs: `DOS/`, `BIOS/`, `BOOT/`, `CMD/`, `DEV/`, `H/`, `INC/`, `LIB/`, `MAPPER/`, `MEMM/`, `MESSAGES/`, `SELECT/`. Top-level `MAKEFILE` orchestrates the build; `RUNME.BAT` / `SETENV.BAT` / `CPY.BAT` are build helpers. |
| `v4.0/src/TOOLS/` | Period build toolchain shipped with the source: `MASM.EXE`, `LINK.EXE`, `NMAKE.EXE`, `EXE2BIN.EXE`, `CL.EXE`, `C1.EXE`/`C2.EXE`/`C3.EXE`, plus helpers (`BUILDMSG`, `EXEFIX`, etc.). |
| `v4.0-ozzie/src/` | 12 beta-era Multitasking-DOS technical PDFs. **No source code.** |
| `.readmes/` | Translated README files and the MS-DOS logo. |
| `LICENSE`, `README.md`, `SECURITY.md` | Repo-level metadata, inherited from upstream. |

## Source conventions (preserve when editing)

When changing or adding to the original sources, stay consistent with how the era wrote code:

- **Filenames are UPPERCASE** (`MSDOS.ASM`, not `msdos.asm`). Match this casing for any new `.ASM`/`.INC`/`.H` files placed alongside originals.
- Code is **MASM-style 8086 assembly**: `;` comments, tab-aligned columns, segment directives (`CSEG`, `DSEG`), `PROC`/`ENDP`, period idioms like self-modifying code, hand-tuned register usage, and segment fixups. Don't bulk-reformat or "modernize" style.
- Whitespace, line endings, and tab widths in original files reflect 1980s tooling. Preserve them in surrounding context when editing — don't normalize on save.
- Don't rename historical identifiers, strip developer initials, or "clean up" terse era-appropriate comments unless that's the explicit task.
- Keep edits to historical sources scoped and obvious; don't bundle drive-by cleanups with substantive changes.

## Build & "running" notes

- **v4.0 has a working build.** It is driven by `v4.0/src/MAKEFILE` (nmake) and uses the toolchain in `v4.0/src/TOOLS/`. It is designed to run inside a period DOS environment (e.g. DOSBox, 86Box, PCem, or real hardware) — **not natively on Linux/macOS**. Don't try to invoke `MASM.EXE` on the host.
- **v1.25 and v2.0 do not ship a build system.** They predate the included toolchain. Reproducing them requires sourcing a period assembler.
- There is **no test suite, linter, formatter, or CI** in this repo (yet). Don't add scaffolding for one unless the user asks.

## How Claude should work in this repo

Common task shapes:

- **Understanding** — explain a routine (e.g., the `INT 21h` dispatcher in `v4.0/src/DOS/`), trace a call path across files, locate where a DOS service is implemented, compare how a feature evolved across v1.25 → v2.0 → v4.0.
- **Editing** — modify or extend `.ASM` sources, fix something for a build experiment, add new files, write supporting tooling. This is a fork; this is fine.
- **Tooling/scaffolding around the sources** — e.g. wrappers, scripts, notes — fine when requested, but don't add it speculatively.

Tool tips:

- `Read` `.ASM` files freely — they're plain text.
- Use `grep` for symbols, interrupt numbers, and macro names. v4.0 in particular benefits from `grep -r` across `v4.0/src/`.
- For large surveys across many files, dispatch an Explore subagent rather than reading hundreds of files inline.
- When citing code, use `path:line` references.

Defaults to keep:

- Don't add modern config files (`.editorconfig`, formatters, GitHub Actions, package manifests, language-server configs) unless explicitly requested.
- Don't push to or open PRs against the upstream `microsoft/MS-DOS` remote. Work happens on this fork.

## License & attribution

Released under the **MIT License** (`LICENSE` at repo root). Microsoft copyright is retained on the original source. Any reproductions or derivative works (including this fork) must preserve the license and copyright notice.

## Pointers

- `README.md` — upstream project description, license, "historical reference" notice.
- `LICENSE` — MIT license text.
- `SECURITY.md` — security reporting (template; the source itself is historical).
- `.readmes/` — translated READMEs.
