# Security

Security is not a phase --- check continuously as you work.

## No Private Data in Code

- Never hardcode passwords, API keys, tokens, or secrets
- Never include real user data, email addresses, phone numbers,
  or personal information in source code, HTML, comments, or
  test fixtures
- Use placeholder values in examples: `user@example.com`,
  `555-0100`, `Jane Doe`
- Store secrets in environment variables or dedicated secrets
  managers; reference them by name, never by value
- If a file might contain sensitive data (`.env`, credentials,
  private keys), do not commit it --- add it to `.gitignore`

## Input Validation and Sanitization

- Validate all input at system boundaries (user input, API
  responses, file reads, URL parameters)
- Sanitize data before rendering in HTML to prevent XSS
- Use parameterized queries for database operations --- never
  string concatenation
- Escape shell arguments when constructing commands

## Continuous Security Checks

- Review every commit for accidentally included secrets or
  private data before pushing
- Check for OWASP Top 10 vulnerabilities in web-facing code
- Use HTTPS for all external URLs in code and documentation
- Validate and sanitize file paths to prevent directory
  traversal
- Apply principle of least privilege for permissions and access
