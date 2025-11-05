# Project Structure

## Repository Layout

```
deepagents/
├── libs/                           # Main package source
│   ├── deepagents/                 # Core package
│   │   ├── __init__.py            # Public API exports
│   │   ├── graph.py               # Main agent creation logic
│   │   ├── middleware/            # Modular middleware components
│   │   │   ├── filesystem.py      # File system tools middleware
│   │   │   ├── subagents.py       # Sub-agent spawning middleware
│   │   │   ├── resumable_shell.py # Shell execution middleware
│   │   │   └── patch_tool_calls.py # Tool call patching utilities
│   │   ├── backends/              # Storage backend implementations
│   │   └── tests/                 # Test suites
│   │       ├── unit_tests/        # Unit tests
│   │       └── integration_tests/ # Integration tests
│   └── deepagents-cli/            # CLI package (separate workspace member)
├── src/deepagents/                # Alternative source location (legacy?)
├── examples/                      # Usage examples and demos
│   ├── research/                  # Basic research agent example
└── pyproject.toml                 # Package configuration and dependencies
```

## Package Organization

### Core Module (`libs/deepagents/`)
- **`__init__.py`**: Exports main public API (`create_deep_agent`, middleware classes)
- **`graph.py`**: Contains the primary `create_deep_agent` factory function
- **`middleware/`**: Modular components that can be composed together

### Middleware Architecture
Each middleware component provides specific capabilities:
- **FilesystemMiddleware**: ls, read_file, write_file, edit_file tools
- **SubAgentMiddleware**: task tool for spawning specialized sub-agents  
- **TodoListMiddleware**: write_todos tool for planning and progress tracking
- **ResumableShellMiddleware**: Shell execution capabilities

### Examples Structure
- Each example is self-contained with its own requirements.txt
- Examples include LangGraph configuration (langgraph.json)

## Development Conventions

### File Naming
- Use snake_case for Python files and directories
- Middleware files named by functionality (e.g., `filesystem.py`, `subagents.py`)
- Test files mirror source structure under `tests/`

### Import Patterns
- Public API exposed through `__init__.py` exports
- Internal imports use relative imports within package
- External dependencies imported at module level

### Configuration Files
- **pyproject.toml**: Single source of truth for package metadata, dependencies, and tool configuration
- **Makefile**: Development workflow automation
- **uv.lock**: Dependency lock file for reproducible builds