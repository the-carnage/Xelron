# JSON Key Extractor — Harbor Task

A medium-difficulty data processing task for the Harbor benchmark framework. An agent reads a JSON server inventory, processes nested fields, and generates a structured plain-text report.

## Task Overview

- **Input**: `/app/input.json` — array of 8 server records with nested config, metrics, tags, datacenter and status fields
- **Output**: `/app/output.txt` — multi-section inventory report

The report covers:

1. Total and active server counts
2. Unique roles (sorted alphabetically)
3. Servers tagged as critical
4. Resource leaders (highest CPU, memory, longest uptime)
5. Datacenter summary with server counts
6. Servers with SSL disabled

## Structure

```
harbor_tasks/json-key-extractor/
├── task.toml              # Metadata (medium, data-processing)
├── instruction.md         # Full task specification
├── environment/
│   ├── Dockerfile         # Python 3.13-slim + jq
│   └── input.json         # 8 nested server records
├── solution/
│   └── solve.sh           # Reference solution using jq
└── tests/
    ├── test.sh            # Test runner with reward output
    └── test_outputs.py    # 11 pytest validation cases
```

## Running Validation

Requires [Harbor](https://github.com/laude-institute/harbor) and Docker.

```bash
# Install harbor dependencies
cd /path/to/harbor && uv sync

# Oracle test (expects 1.0)
uv run harbor run --agent oracle --path /path/to/harbor_tasks/json-key-extractor --job-name test-oracle

# NOP test (expects 0.0)
uv run harbor run --agent nop --path /path/to/harbor_tasks/json-key-extractor --job-name test-nop

# Lint
uvx ruff check harbor_tasks/json-key-extractor
```

## Test Results

| Test   | Expected | Result |
| ------ | -------- | ------ |
| Oracle | 1.0      | 1.0    |
| NOP    | 0.0      | 0.0    |
| Ruff   | Pass     | Pass   |

## Author

Mehebub Alom — mehebye@gmail.com
