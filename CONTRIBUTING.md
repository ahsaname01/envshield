# Contributing to envshield

Thank you for your interest in contributing to `envshield`!

## Development Setup

`envshield` has zero runtime dependencies and uses the Python standard library test runner.

1. Clone the repository and install in editable mode:
   ```bash
   pip install -e .
   ```
2. Run the test suite:
   ```bash
   python -m unittest discover -s tests -v
   ```

## Design Principles

- **Zero external dependencies**: All runtime features must use the Python 3.9+ standard library.
- **Deterministic errors**: Validation errors must report all invalid fields in a single pass with clear remediation hints.
- **Security by default**: Secret fields must never print raw values in `__str__`, `__repr__`, or exception messages.
