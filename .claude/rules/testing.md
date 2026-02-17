# Testing

## Test Pyramid

Aim for a balanced distribution of test types:

- **Unit tests (~80%)** --- Test individual functions and
  modules in isolation. Fast, deterministic, run on the host.
- **Integration tests (~15%)** --- Test interactions between
  modules, real peripherals, or subsystems.
- **End-to-end / system tests (~5%)** --- Test full user
  workflows on target hardware or hardware-in-the-loop (HIL)
  setups.

## Embedded Testing Strategy

For an ESP32-S3 project, structure tests in three tiers:

1. **Host-based tests** --- Pure-logic modules (PID controller,
   profile parser, state machine, data validation) run on the
   development machine using a native test framework (Unity,
   GoogleTest). No hardware required. These form the bulk of
   the test suite.
2. **On-target tests** --- Tests that exercise hardware
   peripherals (SPI, I2C, GPIO, ADC) run on actual ESP32
   hardware. Use PlatformIO's test runner with
   `test_transport = serial`.
3. **Hardware-in-the-loop (HIL)** --- Tests that verify
   closed-loop behavior (e.g., PID controlling a simulated
   boiler) using test fixtures with real sensors and actuators.

Mark each test module with its tier so contributors know what
hardware is needed to run it.

## Best Practices

- Write tests alongside implementation, not as an afterthought
- Run the full existing test suite before committing to catch
  regressions
- Test edge cases and error paths, not just the happy path
- Keep tests independent --- no test should depend on another
  test's state or execution order
- Use descriptive test names that explain what is being
  verified: `test_pid_output_clamps_to_max_when_error_exceeds_range`
- Prefer deterministic tests; avoid flaky tests that depend on
  timing, network, or random values
- Mock external dependencies (APIs, databases, file systems,
  hardware peripherals) in unit tests
- Include both positive tests (it works) and negative tests
  (it fails correctly)
- Tests must be runnable in CI without manual intervention

## Test Structure

Use the Arrange-Act-Assert (AAA) pattern:

1. **Arrange** --- Set up preconditions and inputs
2. **Act** --- Execute the code under test
3. **Assert** --- Verify the expected outcome

Keep each test focused on a single behavior. If a test needs
a long comment to explain what it does, split it into smaller
tests.

## Test Behaviors, Not Implementations

- Test **what** a module does, not **how** it does it
- Verify observable outputs and side effects, not internal
  state
- Avoid asserting on call counts or argument order of mocks
  unless the interaction itself is the behavior under test
- When refactoring does not change behavior, tests should
  not need to change

## Coverage

- Aim for 80% line coverage as a project-wide target
- 100% coverage of safety-critical modules (thermal
  protection, watchdog, fault detection)
- Use coverage reports to find untested paths, not as a goal
  in themselves --- 80% meaningful coverage is better than
  100% superficial coverage

## Anti-Patterns to Avoid

- **The Liar** --- Tests that pass but do not actually verify
  anything meaningful (missing or trivial assertions)
- **Excessive mocking** --- Mocking so much that the test only
  verifies the mock setup, not real behavior
- **Shared mutable state** --- Tests that pass or fail
  depending on execution order due to shared fixtures
- **Testing implementation** --- Asserting on private methods,
  internal data structures, or exact log messages instead of
  observable behavior
- **Copy-paste tests** --- Duplicated test code that drifts
  out of sync; extract shared setup into fixtures or helpers
- **Flaky tests** --- Tests that intermittently fail due to
  timing, network, or random values. Quarantine immediately
  and fix or delete within one sprint.

## Flaky Test Management

- When a test becomes flaky, quarantine it immediately (move
  to a dedicated `quarantine/` directory or mark with a skip
  annotation and a tracking issue)
- Track quarantined tests --- do not let them accumulate
- Fix or delete quarantined tests promptly; do not leave them
  disabled indefinitely

## Static Analysis as Complement

Static analysis catches classes of bugs that tests cannot:

- Use cppcheck for undefined behavior, memory leaks, and
  null pointer dereferences
- Use clang-tidy for modernization, readability, and
  correctness checks
- Consider enabling a subset of MISRA C++ rules for
  safety-critical modules via PlatformIO's `pio check`

## Test Organization

- Mirror the source directory structure in the test directory
- Group related tests logically
- Keep test fixtures and helpers separate from test cases
- Name test files to match source files:
  `src/control/pid.cpp` → `test/control/test_pid.cpp`
