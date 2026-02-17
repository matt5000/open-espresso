# CLAUDE.md - Open Espresso

Project context and instructions for AI-assisted development.

## Project Overview

Open Espresso is an open-source firmware design for retrofitting
Gaggia Classic (pre-2015) espresso machines with an ESP32-S3 MCU.
The project is currently in design phase - the repository contains
design documentation (Markdown, HTML, CSS, SVG) but no firmware
source code yet.

## Repository Structure

```text
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
