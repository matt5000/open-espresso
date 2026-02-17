# Code Style

All code and documentation must conform to the applicable Google
style guides. Always check new and modified files against these
rules before committing.

## Applicable Style Guides

| File Type | Style Guide |
|-----------|-------------|
| HTML/CSS | Google HTML/CSS Style Guide |
| Markdown | Google Markdown Style Guide |
| C/C++ | Google C++ Style Guide (with ESP-IDF deviations) |
| JavaScript | Google JavaScript Style Guide |
| JSON | Google JSON Style Guide |
| Python | Google Python Style Guide |
| Shell | Google Shell Style Guide |

## ESP-IDF Style Deviations

The following deviations from Google C++ Style are required for
ESP-IDF compatibility. When Google and ESP-IDF conflict, follow
ESP-IDF conventions:

| Rule | Google | ESP-IDF (this project) |
|------|--------|-----------------------|
| Indentation | 2 spaces | 4 spaces |
| Method names | `CamelCase` | `snake_case` |
| Header guards | `#ifndef` guards | `#pragma once` |
| File extensions | `.h` | `.hpp` for C++ headers |
| Include ordering | Google 5-tier | ESP-IDF 6-tier |

Document any additional deviations in a `.clang-format` file
at the project root with `BasedOnStyle: Google` and explicit
overrides.

## Enforcement Tools

When available, use automated tools rather than manual review:

| File Type | Formatter | Linter |
|-----------|-----------|--------|
| C/C++ | clang-format | cpplint, cppcheck, clang-tidy |
| Python | Ruff | Ruff, mypy |
| HTML | Prettier | htmlhint |
| CSS | Prettier | Stylelint |
| Markdown | --- | markdownlint |
| Shell | shfmt | ShellCheck |
| JSON | Prettier | --- |
| SVG | --- | manual review |

Formatter configuration files (`.clang-format`,
`pyproject.toml`, `.prettierrc`) are the source of truth for
formatting rules. Prose rules below cover what formatters
cannot enforce (naming, semantics, architecture).

An `.editorconfig` file at the project root provides universal
baseline settings (indent style, charset, trailing whitespace)
across all editors.

## Key Rules to Always Check

### HTML

- Use `<!doctype html>` declaration
- Use UTF-8 encoding (`<meta charset="UTF-8">`)
- Use UTF-8 characters directly instead of HTML entity
  references (e.g., use `---` not `&mdash;`). Exceptions:
  `&amp;` and `&lt;`
- Use valid HTML --- test with the W3C HTML validator
- Use semantic elements for their intended purpose (headings,
  paragraphs, lists, links)
- Separation of concerns: structure (HTML), presentation
  (CSS), behavior (JS) strictly separate
- Do not use inline `style` attributes; use CSS classes instead
- Omit `type` attributes on stylesheets and scripts
- Use 2-space indentation
- Use lowercase for element names and attributes
- Use double quotes for attribute values
- Prefer `class` over `id` for styling; avoid qualifying class
  names with element type selectors
- Provide descriptive `alt` attributes on all images;
  decorative images use `alt=""`
- Use HTTPS for all embedded resources (images, scripts,
  stylesheets)
- Declare page language with `lang` attribute on `<html>`

### CSS

- Use 2-space indentation (not 4-space)
- Expand every rule block --- no single-line rules
- Use meaningful, hyphen-delimited class names reflecting
  purpose (e.g., `.nav-primary`), not presentation
  (e.g., `.blue-text`)
- Use shorthand hex colors where possible (`#fff` not
  `#ffffff`)
- Use shorthand properties where possible (`font`, `padding`,
  `margin`, `border`)
- Use single quotes in CSS (for `font-family`, `content`, etc.)
- Each selector on its own line
- One declaration per line
- End every declaration with a semicolon (including the last)
- Space after property colon, no space before it
- Add a blank line between rule blocks
- No inline styles in HTML --- define CSS classes instead
- Omit units after `0` values (`margin: 0` not `margin: 0px`)
- Include leading zeros for decimal values (`0.5em` not
  `.5em`)
- Avoid `!important` --- if used, add a comment explaining why
  (e.g., accessibility override, print stylesheet)

### Markdown

- Use ATX-style headings (`#`, `##`, `###`)
- One blank line before and after headings
- Single H1 per document as the title
- Wrap lines at 80 characters where practical
- Add language identifiers to fenced code blocks; prefer
  fenced blocks over indented blocks
- Use hyphens (`-`) for unordered lists
- Use lazy numbering (`1.` for all items) in long ordered
  lists to simplify editing
- Use informative link text --- never "click here" or "link"
- Use reference-style links for long URLs or URLs used more
  than once
- Prefer Markdown over raw HTML unless absolutely necessary
- No trailing whitespace; use a trailing backslash (`\`) for
  intentional line breaks
- Surround tables with blank lines

### SVG

- Always define a `viewBox` attribute for responsive scaling
- Include `width` and `height` attributes as fallback
- Use 2-space indentation, consistent with HTML
- Use `<style>` blocks or CSS classes rather than inline
  `style` attributes on individual elements
- Use `<defs>` and `<use>` for repeated elements (arrows,
  connectors, icon shapes)
- Define colors as CSS custom properties or in `<style>` blocks
  for consistency across diagrams
- Add `<title>` and `<desc>` elements for screen reader
  accessibility; use `role="img"` and `aria-labelledby` when
  embedding in HTML
- Use semantic, descriptive `id` and `class` values
- Remove editor-generated metadata, unused namespaces, and
  empty attributes
- Specify `font-family` fallbacks; use web-safe fonts
- Use lowercase-hyphenated file names

## Accessibility (WCAG 2.2 AA)

These rules apply to all HTML documentation pages:

- Maintain logical heading hierarchy (H1 → H2 → H3 --- no
  skipping levels)
- Minimum color contrast ratio: 4.5:1 for normal text, 3:1
  for large text
- All interactive elements must be keyboard-accessible
- Visible focus indicators on all focusable elements; never
  remove `outline` without a visible replacement
- Provide skip-navigation links for repetitive navigation
  blocks
- Declare page language with `lang` attribute on `<html>`
- Interactive targets minimum 24x24 CSS pixels
- Use ARIA attributes only when native HTML semantics are
  insufficient

## When to Check

Run a style check against the applicable Google style guide:

1. Before every commit
2. When reviewing pull requests
3. When adding new files of any type
4. When modifying existing files
