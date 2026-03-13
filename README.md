# Distributed Systems Learning Repository

A comprehensive curriculum for mastering distributed systems through practical implementation.

## Overview

This repository contains a 5-month intensive curriculum for learning distributed systems with Python implementations. It covers storage, stream processing, orchestration, and consensus through labs and exams.

## Prerequisites

- Python 3.10+
- Docker & Docker Compose

## Quick Start

```bash
# Setup environment
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Start services
docker-compose up -d

# Run tests
pytest
```

## Repository Structure

- `distributed_systems_curriculum.md` - Full curriculum (5 phases, 13 chapters)
- `docker-compose.yml` - Local development environment
- `prometheus.yml` - Prometheus configuration
- `labs/` - Implementation labs
- `exams/` - Exam implementations
- `tests/` - Test suites
- `AGENTS.md` - Guidelines for AI agents

## Docker Services

| Service | Port | Description |
|---------|------|-------------|
| Kafka | 9092 | Stream processing |
| Cassandra | 9042 | Distributed database |
| Redis | 6379 | Caching |
| PostgreSQL | 5432 | Relational database |
| etcd | 2379 | Service discovery & consensus |
| Elasticsearch | 9200 | Search & analytics |
| Kibana | 5601 | Elasticsearch UI |
| Prometheus | 9090 | Metrics |
| Grafana | 3000 | Visualization |
| Jaeger | 16686 | Distributed tracing |
| MinIO | 9000 | S3-compatible storage |

Start all services:
```bash
docker-compose up -d
```

Start a specific service:
```bash
docker-compose up -d kafka
```

View logs:
```bash
docker-compose logs -f kafka
```

Stop services:
```bash
docker-compose down
```

## Development Commands

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

### Linting & Type Checking

```bash
# Ruff (linter + formatter)
ruff check .
ruff check --fix .
ruff format .

# MyPy (type checking)
mypy .

# Pre-commit hooks
pre-commit run --all-files
```

## Curriculum Phases

### Phase 1: Foundations (Weeks 1-3)
- Distributed systems fundamentals
- CAP theorem and tradeoffs
- Failure models
- RPC and communication patterns

### Phase 2: Storage & Databases (Weeks 4-7)
- Replication strategies
- Sharding and partitioning
- Distributed transactions
- Consistency models

### Phase 3: Stream Processing (Weeks 8-10)
- Event streaming with Kafka
- Stream processing patterns
- Exactly-once semantics
- Windowing and aggregations

### Phase 4: Orchestration & Operations (Weeks 11-14)
- Container orchestration with Kubernetes
- Service mesh fundamentals
- Service discovery
- Load balancing

### Phase 5: Production Scenarios (Weeks 15-18)
- Deployment strategies
- Observability and monitoring
- Chaos engineering
- Performance optimization

## Resources

- [Curriculum Document](distributed_systems_curriculum.md)
- [Agents Guidelines](AGENTS.md)
- PEP 8 Style Guide
- Python typing documentation

## License

This is a learning repository for educational purposes.
