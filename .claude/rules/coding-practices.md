# Coding Practices

## Variable-Driven Coding

- Use constants or configuration variables instead of magic
  numbers and hardcoded strings
- Define configuration at the top of files or in dedicated
  config files, not scattered through logic
- Use environment variables for values that change between
  environments (dev, staging, prod)
- Name constants descriptively: `MAX_RETRY_ATTEMPTS = 3` not
  `N = 3`
- Centralize shared constants --- do not duplicate the same
  value across files
- Use enums or constant objects for related sets of values

## Documentation Standards

- Use Markdown (`.md`) as the canonical source for all
  documentation
- Create HTML renderings of key documents for readability when
  the project includes HTML documentation
- Keep Markdown as the source of truth; HTML mirrors the content
- Put substantive information in committed files, not just in
  conversation --- knowledge should persist across sessions

## Error Handling

- Handle errors explicitly --- no silent failures
- Log errors with enough context to diagnose the problem
- Never expose stack traces, internal paths, or system details
  to end users
- Use consistent error patterns within a project (exceptions,
  result types, error codes)
- Clean up resources (file handles, connections, locks) in error
  paths

## Dependencies

- Do not add dependencies unless genuinely needed
- Pin dependency versions for reproducible builds
- Prefer well-maintained libraries with active communities
- Check license compatibility before adding a dependency
- Keep dependency count minimal --- standard library first

## Git Hygiene

- Do not commit generated files, build artifacts, or IDE
  configuration
- Keep `.gitignore` up to date for the project's toolchain
- Write commit messages that explain why, not just what
- Review diffs before committing to catch accidental inclusions

## General Practices

- Read existing code before modifying it
- Prefer editing existing files over creating new ones
- Do not leave dead code or commented-out blocks
- No sensitive data in logs or console output
