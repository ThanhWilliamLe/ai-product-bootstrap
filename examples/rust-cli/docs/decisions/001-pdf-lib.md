# D1: PDF Rendering Library

**Status: PENDING**

Which Rust-native PDF library to use for rendering Markdown output.

## Requirement

Must be pure Rust (no C bindings, no unsafe). Must support: fonts (regular, bold, italic, monospace), page layout, text wrapping, basic drawing for table borders.

## Options

### genpdf

- High-level API, built on top of printpdf
- Handles text wrapping and page breaks automatically
- Less control over low-level layout
- Actively maintained, good docs

### printpdf

- Low-level PDF generation
- Full control over positioning, fonts, drawing
- No built-in text wrapping or pagination — must implement manually
- Mature, widely used

### typst

- Full typesetting engine (like LaTeX)
- Very powerful layout and styling
- Heavier dependency, larger binary
- Could use typst as a library crate

## Trade-offs

- genpdf: fastest path to working output, may hit limits on table/complex layout
- printpdf: most control, most implementation work
- typst: most capable, largest dependency footprint

## Decision

Pending. Resolve before starting any rendering work (queue items 4-8).

Constrains: G2 (Render PDF), G3 (Styling support), all renderer code in src/renderer.rs.
