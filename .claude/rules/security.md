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

## Dependency and Supply Chain Security

- Verify dependency names before adding them --- AI-suggested
  package names may reference non-existent packages
  (slopsquatting)
- Pin exact dependency versions in build manifests
- Review changelogs and diffs when upgrading dependencies
- Prefer dependencies with active maintenance and security
  track records
- Check license compatibility before adding any dependency
- Generate and maintain a Software Bill of Materials (SBOM)
  when the project reaches implementation phase

## Embedded and IoT Security

These rules apply when firmware development begins:

- Enable secure boot to prevent unauthorized firmware execution
- Enable flash encryption to protect firmware and data at rest
- Use NVS encryption for stored credentials and sensitive
  configuration
- Disable JTAG and UART debug interfaces in production builds
- Implement secure OTA updates with signature verification and
  anti-rollback protection
- Never ship debug builds or leave debug endpoints enabled in
  release firmware
- Default to secure settings --- require explicit opt-in for
  any insecure configuration

## Web Interface Security

These rules apply to the ESP32 web interface:

- Set Content-Security-Policy (CSP) headers to prevent XSS
- Configure CORS to restrict cross-origin requests to
  expected origins
- Use CSRF tokens for state-changing operations
- Set appropriate cache-control headers for sensitive responses
- Serve all resources over HTTPS when possible; use secure
  WebSocket (WSS) for real-time data

## Cryptography

- Use established cryptographic libraries (e.g., mbedTLS
  bundled with ESP-IDF) --- never implement custom crypto
- Do not use deprecated algorithms (MD5, SHA-1 for security
  purposes, DES, RC4)
- Use sufficient key lengths (AES-128 minimum, RSA-2048
  minimum, Ed25519 preferred for signing)
- Store keys in secure storage (NVS encrypted partition), never
  in source code or plaintext files

## AI-Generated Code Vigilance

- Treat all AI-generated code as untrusted --- review it with
  the same rigor as third-party contributions
- Verify that suggested dependency names correspond to real,
  legitimate packages before adding them
- Check AI-generated code for common CWEs: buffer overflows,
  injection flaws, missing input validation, hardcoded
  credentials, insecure defaults

## Safe Abstractions

- Encapsulate risky operations (hardware register access,
  cryptographic operations, network I/O) in modules with safe
  public APIs
- Expose only validated, type-safe interfaces to the rest of
  the codebase
- Keep unsafe or raw operations private within their modules

## Continuous Security Checks

- Review every commit for accidentally included secrets or
  private data before pushing
- Check for OWASP Top 10 vulnerabilities in web-facing code
- Use HTTPS for all external URLs in code and documentation
- Validate and sanitize file paths to prevent directory
  traversal
- Apply principle of least privilege for permissions and access
- Use secret scanning tools (e.g., Gitleaks) in pre-commit
  hooks when available
- Use static analysis tools (cppcheck, clang-tidy) to catch
  security-relevant bugs early
