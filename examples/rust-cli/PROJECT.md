# md2pdf

CLI tool that converts Markdown files to PDF. For developers who want a fast, single-binary Markdown-to-PDF converter with no runtime dependencies.

## Goals

- G1: Parse Markdown — correctly handle CommonMark input including headings, paragraphs, lists, code blocks, emphasis, tables
- G2: Render PDF — produce valid, well-structured PDF output from parsed Markdown
- G3: Styling support — sensible default styles for headings, body text, code blocks, lists, tables
- G4: CLI interface — intuitive argument handling: input file, output file, options
- G5: Fast — sub-second conversion for typical documents (<100 pages)
- G6: Correct — output matches Markdown semantics; no mangled formatting, no crashes on edge cases
- G7: Distributable — single static binary, GitHub Releases with CI, cross-platform (Linux, macOS, Windows)

## State

Version: 0.1
Phase: building
Last shipped: — (none yet)

## Queue

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 1 | Decide on PDF rendering library (genpdf vs printpdf vs typst) | G2 | decide | — | ready |
| 2 | Scaffold project: Cargo.toml, src/main.rs, CI lint check | G7 | build | 1 | — |
| 3 | Implement Markdown parser (CommonMark via pulldown-cmark) | G1 | build | 2 | — |
| 4 | Implement basic PDF renderer: headings + paragraphs | G2 | build | 3 | — |
| 5 | Add code block rendering with monospace font | G3 | build | 4 | — |
| 6 | Add list rendering (ordered + unordered, nested) | G1, G2 | build | 4 | — |
| 7 | Add table rendering | G1, G2 | build | 4 | — |
| 8 | Add emphasis/strong/inline code rendering | G1, G3 | build | 4 | — |
| 9 | Build integration test harness with fixture Markdown files | G6 | build | 4 | — |
| 10 | Implement CLI argument parsing (clap) | G4 | build | 2 | — |
| 11 | Error handling: friendly messages, non-zero exit codes | G4, G6 | build | 10 | — |
| 12 | GitHub Actions release workflow (build + publish binaries) | G7 | build | 2 | — |

## Decisions

- D1: Pending — PDF rendering library choice — see docs/decisions/001-pdf-lib.md — constrains: G2, G3, all rendering items

## Done

(none yet)
