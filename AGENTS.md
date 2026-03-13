# AGENTS.md - Distributed Systems Learning Repository

## Overview

This repository contains a comprehensive distributed systems curriculum with practical labs and exams. The codebase primarily consists of Python implementations for learning distributed systems concepts.

## Repository Structure

```
.
├── distributed_systems_curriculum.md   # Main curriculum document
├── docker-compose.yml                   # Local development environment
├── prometheus.yml                       # Prometheus configuration
├── labs/                                # Implementation labs
├── exams/                               # Exam implementations
└── tests/                               # Test suites for exams
```

## Build, Lint, and Test Commands

### Python Environment Setup

```bash
# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
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
# Run ruff (linter + formatter)
ruff check .
ruff check --fix .
ruff format .

# Run mypy (type checking)
mypy .

# Run all checks (ruff + mypy + pytest)
make check  # if Makefile exists

# Run pre-commit hooks
pre-commit run --all-files
```

### Docker Services

```bash
# Start all services
docker-compose up -d

# Start specific service
docker-compose up -d kafka

# View logs
docker-compose logs -f kafka

# Stop all services
docker-compose down

# Reset volumes (clean start)
docker-compose down -v
```

## Code Style Guidelines

### General Principles

- Write clean, readable code over clever code
- Prefer explicit over implicit
- Keep functions small and focused (max 50 lines when possible)
- Document "why" not "what" in comments
- Write tests before implementation (TDD for labs)

### Imports

```python
# Standard library imports first
import os
import sys
from typing import List, Dict, Optional
from dataclasses import dataclass

# Third-party imports second
import grpc
from google.protobuf import message

# Local imports last
from .models import User
from .utils import logger

# Avoid wildcard imports
from utils import *  # BAD

# Use absolute imports
from package.module import func  # GOOD
```

### Formatting

- Use 4 spaces for indentation (no tabs)
- Maximum line length: 100 characters
- Use trailing commas in multi-line calls
- Two blank lines between top-level definitions
- One blank line between method definitions

```python
# Good formatting example
def process_items(
    items: List[str],
    callback: Callable[[str], None],
) -> None:
    """Process items with callback."""
    for item in items:
        callback(item)


class DistributedStore:
    """A distributed key-value store."""

    def __init__(self, nodes: List[str]) -> None:
        self.nodes = nodes
        self.cache: Dict[str, str] = {}

    def get(self, key: str) -> Optional[str]:
        """Get value by key."""
        return self.cache.get(key)
```

### Type Annotations

- Always use type annotations for function signatures
- Use `Optional[X]` instead of `X | None` for Python 3.10 compatibility
- Use `List`, `Dict`, `Tuple` from typing (or builtins for Python 3.9+)
- Add return type annotations to all functions

```python
# Good type annotations
def calculate_replica_count(total_nodes: int, replication_factor: int) -> int:
    return min(total_nodes, replication_factor)

def get_nodes(self) -> List[str]:
    return self._nodes.copy()

def process_batch(self, items: List[Item]) -> Dict[str, int]:
    results: Dict[str, int] = {}
    for item in items:
        results[item.id] = item.process()
    return results
```

### Naming Conventions

- **Variables/functions**: `snake_case` (e.g., `replication_factor`, `get_leader_node`)
- **Classes**: `PascalCase` (e.g., `RaftNode`, `DistributedCache`)
- **Constants**: `UPPER_SNAKE_CASE` (e.g., `MAX_RETRIES`, `DEFAULT_TIMEOUT`)
- **Private methods**: `_leading_underscore` (e.g., `_elect_leader`)
- **Modules**: `snake_case` (e.g., `raft_node.py`, `test_utils.py`)

```python
# Examples
MAX_REPLICATION_FACTOR = 3
DEFAULT_TIMEOUT_SECONDS = 5

class RaftConsensus:
    def __init__(self, node_id: str) -> None:
        self.node_id = node_id
        self._leader: Optional[str] = None

    def _become_leader(self) -> None:
        """Internal method to transition to leader state."""
        pass
```

### Error Handling

- Use custom exceptions for domain-specific errors
- Catch specific exceptions, not bare `except:`
- Include context in error messages
- Log errors before re-raising
- Use context managers for resource cleanup

```python
class ReplicationError(Exception):
    """Raised when replication fails."""
    pass


class NodeUnavailableError(ReplicationError):
    """Raised when a node is unavailable."""
    pass


def replicate_write(self, key: str, value: str) -> None:
    """Replicate write to follower nodes."""
    for node in self.followers:
        try:
            node.send(key, value)
        except ConnectionError as e:
            logger.error(f"Failed to replicate to {node}: {e}")
            raise NodeUnavailableError(f"Node {node} unavailable") from e
```

### Logging

- Use the `logging` module, not print statements
- Use appropriate log levels: DEBUG, INFO, WARNING, ERROR, CRITICAL
- Include contextual information in log messages
- Use f-strings for log messages

```python
import logging

logger = logging.getLogger(__name__)


def join_cluster(self, node_id: str) -> None:
    """Join the cluster as a new node."""
    logger.info(f"Node {node_id} joining cluster with {len(self.peers)} peers")
    
    if len(self.peers) >= self.max_nodes:
        logger.warning(f"Cluster at capacity: {len(self.peers)}/{self.max_nodes}")
        raise ClusterFullError("Cannot add more nodes")
    
    self._send_join_request(node_id)
    logger.info(f"Node {node_id} successfully joined")
```

### Testing Guidelines

- Use `pytest` as the test framework
- Write descriptive test names: `test_<method>_<scenario>_<expected>`
- Use fixtures for common setup
- Mock external dependencies
- Test edge cases and error conditions

```python
import pytest
from unittest.mock import Mock, patch


class TestRaftElection:
    """Tests for Raft leader election."""

    @pytest.fixture
    def node(self) -> RaftNode:
        """Create a Raft node for testing."""
        return RaftNode(node_id="node-1", peers=["node-2", "node-3"])

    def test_becomes_candidate_on_timeout(self, node) -> None:
        """Node should become candidate when election timeout occurs."""
        node._handle_election_timeout()
        
        assert node.state == State.CANDIDATE
        assert node.votes_received == 1  # Votes for self

    def test_election_wins_with_majority(self, node) -> None:
        """Node should win election with majority of votes."""
        node.state = State.CANDIDATE
        node.request_vote("node-2")  # Vote from node-2
        node.request_vote("node-3")  # Vote from node-3
        
        assert node.state == State.LEADER

    def test_election_timeout_resets_on_receive_message(self, node) -> None:
        """Election timeout should reset when receiving from leader."""
        node.state = State.FOLLOWER
        node._last_contact = 0
        
        node.receive_message(Message(type=MessageType.APPEND_ENTRIES))
        
        assert node._last_contact > 0
```

### Concurrency and Async

- Use `asyncio` for async I/O operations
- Use `threading` or `multiprocessing` for CPU-bound parallelism
- Always use locks when accessing shared state
- Prefer `asyncio.create_task()` over threads for I/O-bound work

```python
import asyncio
import threading
from typing import Any, Dict
from threading import Lock


class ThreadSafeCounter:
    """Thread-safe counter implementation."""

    def __init__(self) -> None:
        self._count: int = 0
        self._lock = Lock()

    def increment(self) -> int:
        with self._lock:
            self._count += 1
            return self._count

    async def async_increment(self) -> int:
        await asyncio.sleep(0)  # Yield to event loop
        return self.increment()
```

### Documentation

- Use docstrings for all public classes and functions
- Follow Google docstring format
- Include type hints in docstrings (redundant but helpful)
- Document exceptions that can be raised

```python
def shard_key(key: str, num_shards: int) -> int:
    """Calculate shard index for a given key.
    
    Uses consistent hashing to distribute keys across shards.
    
    Args:
        key: The key to hash.
        num_shards: Total number of shards.
    
    Returns:
        Shard index in range [0, num_shards).
    
    Raises:
        ValueError: If num_shards is not positive.
    """
    if num_shards <= 0:
        raise ValueError("num_shards must be positive")
    
    return hash(key) % num_shards
```

### File Organization

- One class per file for large classes
- Group related functions in modules
- Use `__init__.py` to mark packages
- Keep test files in `tests/` directory with `test_` prefix

```
src/
├── raft/
│   ├── __init__.py
│   ├── node.py          # RaftNode class
│   ├── log.py           # RaftLog class
│   ├── state.py         # State enums
│   └── messages.py      # Message types
└── tests/
    ├── test_raft/
    │   ├── test_node.py
    │   └── test_log.py
    └── conftest.py
```

## Continuous Integration

When CI is configured, tests will run:
- On every push to main
- On all pull requests
- With Python 3.10, 3.11, 3.12

## Notes for Agents

1. **Always run tests** before marking any exam as complete
2. **Use type hints** - they're critical for distributed systems code
3. **Handle failures gracefully** - distributed systems must handle node failures
4. **Document your design decisions** - especially tradeoffs
5. **Test with real failures** - inject network partitions, node crashes
6. **Keep implementations simple** - focus on correctness over optimization

## Resources

- Curriculum: `distributed_systems_curriculum.md`
- Docker services: `docker-compose.yml`
- Python style: PEP 8 + ruff defaults
- Type hints: mypy documentation
