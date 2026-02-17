# Coding Practices

## Design Principles

Apply these principles in order of priority:

- **KISS** --- Keep it simple. Choose the simplest solution
  that solves the problem.
- **YAGNI** --- Do not build for hypothetical future
  requirements. Three similar lines of code are better than a
  premature abstraction.
- **DRY** --- Do not repeat yourself, but only extract common
  code when the repetition is proven and stable. Premature DRY
  creates coupling.
- **Single Responsibility** --- Each module, class, or function
  should have one reason to change.
- **Encapsulation** --- Hide implementation details behind
  stable interfaces. Prefer deep modules with small public
  APIs over shallow modules that expose internals.

## Naming Conventions

Follow Google C++ naming with ESP-IDF adaptations:

- **Types** (classes, structs, enums, typedefs): `CamelCase`
- **Functions and methods**: `snake_case` (ESP-IDF convention)
- **Variables**: `snake_case`
- **Constants and enum values**: `kCamelCase` or
  `UPPER_SNAKE_CASE`
- **File-local static variables**: prefix with `s_`
  (e.g., `s_instance_count`)
- **Namespaces**: `lower_snake_case`
- **Component prefixes**: Use a short component prefix for
  public symbols to avoid collisions
  (e.g., `ctrl_pid_compute()`, `hw_boiler_set_power()`)

Choose names that are self-documenting. A reader should
understand purpose without checking the definition.

## Function and Module Design

- Keep functions short and focused --- if a function needs a
  comment explaining a section, that section is a candidate
  for extraction
- Limit function complexity: aim for a cyclomatic complexity
  of 10 or less
- Prefer pure functions (no side effects) where possible ---
  they are easier to test and reason about
- Limit function parameters to 4 or fewer; group related
  parameters into a struct or config object
- Functions should do one thing at one level of abstraction

## Comments

- Explain **why**, not **what** --- code should be
  self-explanatory for what it does
- Document all public interfaces: purpose, parameters, return
  values, error conditions
- Use `TODO(username): description` format for open items,
  with enough context that someone else can act on it
- Do not add trivial comments that restate the code
  (e.g., `// increment counter` before `counter++`)
- Keep comments up to date --- a stale comment is worse than
  no comment
- Remove commented-out code; use version control to retrieve
  old code

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

## Embedded-Specific Practices

These rules apply when firmware development begins:

### Memory Management

- Prefer stack allocation over heap allocation
- Use `heap_caps_malloc()` for DMA-capable or IRAM allocations
- Avoid dynamic allocation in real-time critical paths and ISRs
- Track heap high-water marks to verify resource budgets from
  the design document

### ISR and Concurrency Safety

- Mark variables shared between ISR and main context as
  `volatile`
- Keep ISR handlers minimal --- defer work to tasks via queues
  or task notifications
- Do not call blocking functions, heap allocation, or logging
  from ISR context
- Avoid virtual function calls in IRAM-safe interrupt handlers
  (vtables are in flash)

### C++ on ESP32

- Avoid `<iostream>` --- it adds ~200 KB binary size; use
  ESP-IDF logging (`ESP_LOGI`, `ESP_LOGW`, etc.)
- Avoid exceptions in real-time critical code paths (exception
  handling has non-deterministic timing)
- RTTI is disabled by default in ESP-IDF; avoid `dynamic_cast`
  and `typeid`
- Avoid `setjmp`/`longjmp` in C++ (they skip destructors)
- Mark file-local functions and variables as `static` to help
  the linker discard unused code
- Use `extern "C"` guards in all C header files for C++
  compatibility

### Hardware Abstraction

- Wrap hardware access behind HAL interfaces so pure-logic
  modules can be tested on the host
- Never access hardware registers directly from application
  logic --- go through driver modules

## Logging

- Use ESP-IDF log levels consistently:
  - `ESP_LOGE` --- Errors that require immediate attention
  - `ESP_LOGW` --- Unexpected but recoverable conditions
  - `ESP_LOGI` --- Significant operational events
  - `ESP_LOGD` --- Debugging information (disabled in release)
  - `ESP_LOGV` --- Verbose tracing (disabled in release)
- Include enough context to diagnose without reproducing:
  function name, relevant variable values, error codes
- Never log sensitive data (passwords, keys, user data)
- Do not log in tight loops or ISR context --- it blocks
  execution and can fill buffers
- Use structured, parseable log formats when possible

## Error Handling

- Handle errors explicitly --- no silent failures
- Log errors with enough context to diagnose the problem
- Never expose stack traces, internal paths, or system details
  to end users
- Use consistent error patterns within a project (error codes
  for C, result types for C++)
- Clean up resources (file handles, connections, locks) in
  error paths --- use RAII in C++ where possible
- For safety-critical errors (over-temperature, sensor
  failure), always fail to a safe state as defined in the
  design document

## Documentation Standards

- Use Markdown (`.md`) as the canonical source for all
  documentation
- Create HTML renderings of key documents for readability when
  the project includes HTML documentation
- Keep Markdown as the source of truth; HTML mirrors the
  content
- Put substantive information in committed files, not just in
  conversation --- knowledge should persist across sessions

## Dependencies

- Do not add dependencies unless genuinely needed
- Pin exact dependency versions for reproducible builds
- Use lockfiles (`platformio.ini` lock, `package-lock.json`)
  to ensure deterministic installs
- Prefer well-maintained libraries with active communities
- Check license compatibility before adding a dependency
- Keep dependency count minimal --- standard library first
- Track dependencies in a bill-of-materials when the project
  reaches release phase

## Code Review

Before approving or merging any change, verify:

1. **Correctness** --- Does the code do what it claims?
2. **Design** --- Is the approach appropriate? Does it fit the
   architecture?
3. **Readability** --- Can another developer understand it
   without the author explaining?
4. **Tests** --- Are the changes tested? Do existing tests
   still pass?
5. **Naming** --- Are names clear, consistent, and descriptive?
6. **Style** --- Does the code follow the project style guide?
7. **Security** --- Are inputs validated? Are secrets absent?
8. **Documentation** --- Are public interfaces documented? Are
   design docs updated?

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
- Profile before optimizing --- never sacrifice safety or
  readability for unmeasured performance gains
- Reference resource budgets from the design document when
  making performance decisions
