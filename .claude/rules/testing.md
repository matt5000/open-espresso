# Testing

## Best Practices

- Write tests alongside implementation, not as an afterthought
- Run the full existing test suite before committing to catch
  regressions
- Test edge cases and error paths, not just the happy path
- Keep tests independent --- no test should depend on another
  test's state or execution order
- Use descriptive test names that explain what is being verified
- Prefer deterministic tests; avoid flaky tests that depend on
  timing, network, or random values
- Mock external dependencies (APIs, databases, file systems) in
  unit tests
- Include both positive tests (it works) and negative tests
  (it fails correctly)

## Test Organization

- Mirror the source directory structure in the test directory
- Group related tests logically
- Keep test fixtures and helpers separate from test cases
