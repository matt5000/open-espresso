# Code Style

All code and documentation must conform to the applicable Google
style guides. Always check new and modified files against these
rules before committing.

## Applicable Style Guides

| File Type | Style Guide |
|-----------|-------------|
| HTML/CSS | Google HTML/CSS Style Guide |
| Markdown | Google Markdown Style Guide |
| C/C++ | Google C++ Style Guide |
| JavaScript | Google JavaScript Style Guide |
| JSON | Google JSON Style Guide |
| Python | Google Python Style Guide |
| Shell | Google Shell Style Guide |

## Key Rules to Always Check

### HTML

- Use UTF-8 encoding (`<meta charset="UTF-8">`)
- Use UTF-8 characters directly instead of HTML entity
  references (e.g., use `---` not `&mdash;`). Exceptions:
  `&amp;` and `&lt;`
- Do not use inline `style` attributes; use CSS classes instead
- Omit `type` attributes on stylesheets and scripts
- Use lowercase for element names and attributes
- Use double quotes for attribute values
- Provide `alt` attributes on all images
- Meet WCAG 2.1 AA accessibility standards

### CSS

- Use 2-space indentation (not 4-space)
- Expand every rule block --- no single-line rules
- Use shorthand hex colors where possible (`#fff` not
  `#ffffff`)
- Use single quotes in CSS (for `font-family`, `content`, etc.)
- Each selector on its own line
- One declaration per line
- Add a blank line between rule blocks
- No inline styles in HTML --- define CSS classes instead

### Markdown

- Use ATX-style headings (`#`, `##`, `###`)
- One blank line before and after headings
- Wrap lines at 80 characters where practical
- Add language identifiers to fenced code blocks
- Use hyphens (`-`) for unordered lists

## When to Check

Run a style check against the applicable Google style guide:

1. Before every commit
2. When reviewing pull requests
3. When adding new files of any type
4. When modifying existing files
