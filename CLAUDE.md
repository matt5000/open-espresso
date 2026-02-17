# CLAUDE.md — Open Espresso

Project context and instructions for AI-assisted development.

## Project Overview

Open Espresso is an open-source firmware design for retrofitting
Gaggia Classic (pre-2015) espresso machines with an ESP32-S3 MCU.
The project is currently in design phase — the repository contains
design documentation (Markdown, HTML, CSS, SVG) but no firmware
source code yet.

## Google Style Guide Compliance

**All code and documentation in this repository must conform to the
applicable Google style guides.** Always check new and modified files
against these rules before committing.

### Applicable Style Guides

| File Type | Style Guide |
|-----------|-------------|
| HTML | [Google HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html) |
| CSS | [Google HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html) |
| Markdown | [Google Markdown Style Guide](https://google.github.io/styleguide/docguide/style.html) |
| C/C++ | [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html) (when firmware code is added) |
| JavaScript | [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html) (when web app code is added) |
| JSON | [Google JSON Style Guide](https://google.github.io/styleguide/jsoncstyleguide.xml) (when data files are added) |

### Key Rules to Always Check

#### HTML
- Use UTF-8 encoding (`<meta charset="UTF-8">`)
- Use UTF-8 characters directly instead of HTML entity references
  (e.g., use `—` not `&mdash;`). Exceptions: `&amp;` and `&lt;`
- Do not use inline `style` attributes (separation of concerns);
  use CSS classes instead
- Omit `type` attributes on stylesheets and scripts
- Use lowercase for element names and attributes
- Use double quotes for attribute values
- Provide `alt` attributes on all images

#### CSS
- Use 2-space indentation (not 4-space)
- Expand every rule block — no single-line rules
- Use shorthand hex colors where possible (`#fff` not `#ffffff`)
- Use single quotes in CSS (for `font-family`, `content`, etc.)
- Each selector on its own line
- One declaration per line
- Add a blank line between rule blocks
- Separate `rgba()` arguments with commas and spaces
- No inline styles in HTML — define CSS classes instead

#### Markdown
- Use ATX-style headings (`#`, `##`, `###`)
- One blank line before and after headings
- Wrap lines at 80 characters where practical
- Add language identifiers to fenced code blocks
- Use hyphens (`-`) for unordered lists

### When to Check

Run a style check against the applicable Google style guide:
1. Before every commit
2. When reviewing pull requests
3. When adding new files of any type
4. When modifying existing files

## Repository Structure

```
open-espresso/
  CLAUDE.md                      This file
  README.md                      Project overview
  docs/
    DESIGN_DOCUMENT.md           Canonical design document (154KB)
    DESIGN_DOCUMENT_GUIDELINES.md  IEEE 1016/29148 writing guide
    DESIGN_DOCUMENT.html         Multi-page HTML design document
    current-state.html
    hardware.html
    software.html
    safety.html
    data-model.html
    requirements.html
    interfaces.html
    decisions.html
    css/design-document.css      Stylesheet for HTML docs
    diagrams/*.svg               Architecture diagrams (29 files)
```

## Design Document

The canonical source of truth is `docs/DESIGN_DOCUMENT.md`. The HTML
files are a multi-page rendering of the same content. Changes to
design content should be made in the Markdown source first, then
reflected in the HTML pages.

The design document follows IEEE 1016-2009 (Software Design
Descriptions) and IEEE 29148-2018 (Requirements Engineering).
See `docs/DESIGN_DOCUMENT_GUIDELINES.md` for the writing conventions.

## Testing and Verification

The design document includes a formal test plan (Section 18) and
traceability matrix linking requirements to design modules to tests.
Style compliance is verified as part of the document review checklist
(see `docs/DESIGN_DOCUMENT_GUIDELINES.md`, "Review Checklist").
