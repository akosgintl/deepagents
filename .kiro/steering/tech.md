# Technology Stack

## Build System & Package Management

- **Package Manager**: `uv` (modern Python package manager)
- **Build System**: setuptools with pyproject.toml configuration
- **Python Version**: >=3.11,<4.0
- **Workspace**: Multi-package workspace with libs/deepagents and libs/deepagents-cli

## Core Dependencies

- **LangChain Stack**: langchain, langchain-core, langchain-anthropic
- **LLM Integration**: Anthropic Claude (default: claude-sonnet-4-5-20250929)
- **Graph Framework**: LangGraph for agent orchestration
- **Pattern Matching**: wcmatch for filesystem operations

## Development Tools

- **Linting & Formatting**: ruff (line length: 150)
- **Type Checking**: mypy with strict mode
- **Testing**: pytest with coverage reporting
- **Documentation**: Google-style docstrings

## Common Commands

### Development Setup
```bash
# Install dependencies
uv sync --all-groups

# Install package in development mode
uv pip install -e .
```

### Code Quality
```bash
# Lint code
make lint

# Format code  
make format

# Lint specific package
make lint_package

# Lint only changed files
make lint_diff
```

### Testing
```bash
# Run unit tests
make test

# Run integration tests  
make integration_test

# Run tests with coverage
uv run pytest libs/deepagents/tests/unit_tests --cov=deepagents --cov-report=term-missing
```

### Building & Publishing
```bash
# Build package
uv run python -m build

# Publish to PyPI
uv run twine upload dist/*
```

## Architecture Notes

- **Middleware Pattern**: Core functionality implemented as composable middleware
- **Async Support**: Both sync and async agent creation supported
- **MCP Integration**: Compatible with Model Context Protocol tools via langchain-mcp-adapters
- **LangGraph Native**: Agents are LangGraph graphs, supporting streaming, HITL, memory, and studio