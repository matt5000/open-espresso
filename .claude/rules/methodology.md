# Methodology

Follow this sequence for all non-trivial tasks:

1. **Research** --- Understand the problem, read relevant code,
   and gather context before proposing solutions
2. **Plan** --- Outline the approach and define acceptance
   criteria; get approval before large changes
3. **Implement** --- Write the code or content
4. **Test** --- Verify changes against the acceptance criteria
   from step 2; run the full existing test suite
5. **Review** --- Check your own diff for correctness, style
   compliance, security issues, and accidental inclusions
   before committing
6. **Document** --- Update or create documentation to reflect
   changes

Do not skip steps. For trivial tasks (single-line fixes, typos),
steps 1--2 can be implicit.

## Test-Driven Development

When requirements are clear and testable, prefer writing tests
before implementation:

1. Write a failing test that defines the expected behavior
2. Write the minimum code to make the test pass
3. Refactor while keeping tests green

TDD is especially valuable for bug fixes (write a test that
reproduces the bug first) and for pure-logic modules that can
run on the host without hardware.

## Safety-Critical Exception

For code that touches safety-critical paths (thermal protection,
watchdog, fault detection, actuator control), never treat a
change as trivial. Always follow the full sequence regardless
of change size.
