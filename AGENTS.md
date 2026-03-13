# AGENTS.md - Distributed Systems Learning Repository

## Overview

This repository contains a distributed systems curriculum with Python implementations for labs and exams.

## Repository Structure

```
.
├── distributed_systems_curriculum.md   # Full curriculum (5 phases, 13 chapters)
├── docker-compose.yml                   # Local development environment
├── prometheus.yml                       # Prometheus configuration
├── README.md                            # Project overview
├── AGENTS.md                            # This file
├── books/                               # Recommended reading
├── labs/                                # Implementation labs
├── exams/                               # Exam implementations
└── tests/                               # Test suites
```

## Build, Lint, and Test Commands

### Python Environment Setup

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### Running Tests

```bash
# Run all tests
pytest

# Run a single test file
pytest tests/test_exam_1.py

# Run a specific test
pytest tests/test_exam_1.py::test_replication_works

# Run with verbose output
pytest -v

# Run with coverage
pytest --cov=. --cov-report=html

# Run tests matching a pattern
pytest -k "replication"
```

### Linting and Code Quality

```bash
ruff check .
ruff check --fix .
ruff format .
mypy .
pre-commit run --all-files
```

### Docker Services

```bash
docker-compose up -d
docker-compose up -d kafka
docker-compose logs -f kafka
docker-compose down
docker-compose down -v
```

## Code Style Guidelines

### General Principles

- Write clean, readable code over clever code
- Prefer explicit over implicit
- Keep functions small and focused (max 50 lines)
- Document "why" not "what" in comments

### Imports

- Standard library first, then third-party, then local
- Use absolute imports, avoid wildcard imports

### Formatting

- 4 spaces for indentation (no tabs)
- Maximum line length: 100 characters
- Use trailing commas in multi-line calls
- Two blank lines between top-level definitions

### Type Annotations

- Always use type annotations for function signatures
- Use `Optional[X]` instead of `X | None`
- Add return type annotations to all functions

### Naming Conventions

- **Variables/functions**: `snake_case`
- **Classes**: `PascalCase`
- **Constants**: `UPPER_SNAKE_CASE`
- **Private methods**: `_leading_underscore`

### Error Handling

- Use custom exceptions for domain-specific errors
- Catch specific exceptions, not bare `except:`
- Include context in error messages

### Logging

- Use `logging` module, not print statements
- Use appropriate log levels: DEBUG, INFO, WARNING, ERROR, CRITICAL

### Testing Guidelines

- Use `pytest` as the test framework
- Write descriptive test names: `test_<method>_<scenario>_<expected>`
- Use fixtures for common setup

### Concurrency and Async

- Use `asyncio` for async I/O operations
- Always use locks when accessing shared state

### File Organization

- One class per file for large classes
- Use `__init__.py` to mark packages
- Keep test files in `tests/` with `test_` prefix

### Documentation

- Use docstrings for all public classes and functions
- Follow Google docstring format
- Document exceptions that can be raised

## Notes for Agents

1. **Always run tests** before marking any exam as complete
2. **Use type hints** - critical for distributed systems code
3. **Handle failures gracefully** - distributed systems must handle node failures
4. **Test with real failures** - inject network partitions, node crashes
5. **Keep implementations simple** - focus on correctness over optimization
6. **Use fixtures for test setup** - especially for async distributed tests

## Resources

- Curriculum: `distributed_systems_curriculum.md`
- Books: `./books/ddia-book.pdf`
- Docker services: `docker-compose.yml`
- Python style: PEP 8 + ruff defaults
