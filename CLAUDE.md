# CLAUDE.md

Guidance for Claude (and other AI assistants) working in this repository.

## Project overview

This repo is Microsoft's published **historical source release of MS-DOS**:

- **v1.25** and **v2.0** — original source and period binaries, originally shared via the Computer History Museum (March 2014).
- **v4.0** — source for the IBM/Microsoft jointly developed MS-DOS 4.00, including a working `nmake`-based build and the period MASM/LINK toolchain.
- **v4.0-ozzie** — beta-era documentation PDFs only (no source).

The code is overwhelmingly **8086 assembly (MASM)**. The repo is preserved for reference, study, and experimentation — it is **not an actively maintained codebase**.

## Critical constraint: this is a static archive

From `README.md`:

> The source files in this repo are for historical reference and will be kept static, so please **don't send** Pull Requests suggesting any modifications to the source files, but feel free to fork this repo and experiment.

Default behavior in this repo:

- **Do not modify the historical source files** (`.ASM`, `.INC`, `.H`, `MAKEFILE`, batch scripts, binaries) without an explicit, scoped request from the user.
- If the user asks for a change to historical sources, confirm intent first and frame it as a fork/experiment, not a contribution upstream.
- Adding *new* top-level files that aid navigation (notes, this file, a personal scratch dir) is fine when asked.
- Treat `.EXE`, `.COM`, `.OVR`, and `.PDF` as opaque artifacts — don't try to edit them.

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
| `LICENSE`, `README.md`, `SECURITY.md` | Repo-level metadata. |

## Source conventions (do not "correct" these)

- **Filenames are UPPERCASE** (`MSDOS.ASM`, not `msdos.asm`). Preserve casing in any reference, link, or new file you create.
- Code is **MASM-style 8086 assembly**: `;` comments, tab-aligned columns, segment directives (`CSEG`, `DSEG`), `PROC`/`ENDP`, period idioms like self-modifying code, hand-tuned register usage, and segment fixups. Don't reformat or "modernize" style.
- Whitespace, line endings, and tab widths reflect 1980s tooling. Don't normalize them.
- Comments and identifiers occasionally reflect their era (developer initials, terse abbreviations). Leave them as written.

## Build & "running" notes

- **v4.0 has a working build.** It is driven by `v4.0/src/MAKEFILE` (nmake) and uses the toolchain in `v4.0/src/TOOLS/`. It is designed to run inside a period DOS environment (e.g. DOSBox, 86Box, PCem, or real hardware) — **not natively on Linux/macOS**. Don't try to invoke `MASM.EXE` on the host.
- **v1.25 and v2.0 do not ship a build system.** They predate the included toolchain. Reproducing them requires sourcing a period assembler.
- There is **no test suite, linter, formatter, or CI** in this repo. Don't invent one or add scaffolding for one unless the user explicitly asks.

## How Claude should work in this repo

The default task shape here is **understanding**, not changing:

- Explain what a routine does (e.g., the `INT 21h` dispatcher in `v4.0/src/DOS/`).
- Trace a call path across files; locate where a DOS service is implemented.
- Summarize a file or compare how a feature evolved between v1.25 → v2.0 → v4.0.
- Quote short excerpts with `path:line` references.

Tool tips:

- `Read` `.ASM` files freely — they're plain text.
- Use `grep` for symbols, interrupt numbers, and macro names. v4.0 in particular benefits from `grep -r` across `v4.0/src/`.
- For large surveys across many files, dispatch an Explore subagent rather than reading hundreds of files inline.

When the user *does* ask for edits:

- Confirm whether they want changes to the historical source itself (rare) or to a working copy / experiment / annotation file (more common).
- Keep diffs minimal and obvious. Don't bundle unrelated cleanups.
- Don't add modern config files (`.editorconfig`, formatters, GitHub Actions, package manifests, language-server configs) unless explicitly requested.

## License & attribution

Released under the **MIT License** (`LICENSE` at repo root). Microsoft copyright is retained on the original source. Any reproductions or derivative works must preserve the license and copyright notice.

## Pointers

- `README.md` — project description, license, "historical reference" notice.
- `LICENSE` — MIT license text.
- `SECURITY.md` — security reporting (template; the source itself is historical).
- `.readmes/` — translated READMEs.
