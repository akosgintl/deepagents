# DeepAgents As-Is Requirements Document

## Introduction

DeepAgents is a Python package that implements "deep agents" - LLM-powered agents that go beyond simple tool-calling loops to handle complex, multi-step tasks through planning, context management, and hierarchical task delegation. This specification documents the current state (As-Is) of the DeepAgents system, capturing its existing capabilities, architecture, and functionality.

## Glossary

- **Deep_Agent**: An LLM-powered agent created via create_deep_agent that includes planning, filesystem, and subagent capabilities
- **Middleware**: Composable components that add functionality to agents (planning, filesystem, subagents, etc.)
- **Backend**: Storage mechanism for file operations (StateBackend, StoreBackend, FilesystemBackend, CompositeBackend)
- **SubAgent**: Specialized agent spawned by the main agent for isolated task execution
- **Planning_Tool**: The write_todos tool that enables task decomposition and progress tracking
- **Filesystem_Tools**: Set of six tools (ls, read_file, write_file, edit_file, glob, grep) for file operations
- **Task_Tool**: Tool that enables spawning and invoking subagents
- **LangGraph**: The underlying graph framework used for agent orchestration
- **HITL**: Human-in-the-loop functionality for tool approval workflows

## Requirements

### Requirement 1: Core Agent Creation

**User Story:** As a developer, I want to create deep agents with enhanced capabilities, so that I can build agents that handle complex multi-step tasks effectively.

#### Acceptance Criteria

1. WHEN a developer calls create_deep_agent, THE Deep_Agent SHALL return a compiled LangGraph graph with default middleware
2. THE Deep_Agent SHALL use Claude Sonnet 4.5 as the default model when no model is specified
3. THE Deep_Agent SHALL include TodoListMiddleware, FilesystemMiddleware, and SubAgentMiddleware by default
4. THE Deep_Agent SHALL accept custom models, tools, system prompts, and middleware as parameters
5. THE Deep_Agent SHALL set recursion limit to 1000 for complex task execution

### Requirement 2: Planning and Task Management

**User Story:** As an agent, I want to break down complex tasks into manageable steps, so that I can track progress and adapt plans dynamically.

#### Acceptance Criteria

1. THE Deep_Agent SHALL provide a write_todos tool for task decomposition
2. WHEN the agent encounters multi-step tasks, THE Deep_Agent SHALL use the Planning_Tool to create structured todo lists
3. THE Deep_Agent SHALL update todo lists as tasks progress and new information emerges
4. THE Deep_Agent SHALL maintain todo state across conversation turns
5. THE Deep_Agent SHALL include planning instructions in the system prompt

### Requirement 3: Filesystem Operations

**User Story:** As an agent, I want to manage files and context through a filesystem interface, so that I can handle large amounts of data without context overflow.

#### Acceptance Criteria

1. THE Deep_Agent SHALL provide six Filesystem_Tools: ls, read_file, write_file, edit_file, glob, grep
2. WHEN performing file operations, THE Deep_Agent SHALL validate and normalize all file paths for security
3. THE Deep_Agent SHALL support multiple storage backends (StateBackend, StoreBackend, FilesystemBackend, CompositeBackend)
4. THE Deep_Agent SHALL handle large tool results by automatically saving them to filesystem when exceeding token limits
5. THE Deep_Agent SHALL provide pagination support for reading large files with offset and limit parameters

### Requirement 4: Subagent Management

**User Story:** As an agent, I want to spawn specialized subagents for complex tasks, so that I can isolate context and delegate work effectively.

#### Acceptance Criteria

1. THE Deep_Agent SHALL provide a Task_Tool for spawning subagents
2. THE Deep_Agent SHALL include a general-purpose subagent by default
3. WHEN spawning subagents, THE Deep_Agent SHALL pass filtered state excluding messages and todos
4. THE Deep_Agent SHALL support custom subagent specifications with name, description, system_prompt, tools, model, and middleware
5. THE Deep_Agent SHALL support pre-compiled subagents via CompiledSubAgent interface

### Requirement 5: Middleware Architecture

**User Story:** As a developer, I want to extend agent functionality through composable middleware, so that I can customize agent behavior for specific use cases.

#### Acceptance Criteria

1. THE Deep_Agent SHALL support custom middleware in addition to default middleware
2. THE Deep_Agent SHALL apply middleware in the correct order: default middleware first, then custom middleware
3. THE Deep_Agent SHALL support middleware that adds tools, modifies system prompts, and wraps model calls
4. THE Deep_Agent SHALL include HumanInTheLoopMiddleware when interrupt_on configuration is provided
5. THE Deep_Agent SHALL support middleware state management through typed state schemas

### Requirement 6: Storage Backend System

**User Story:** As a developer, I want flexible storage options for agent files, so that I can choose between ephemeral, persistent, and hybrid storage strategies.

#### Acceptance Criteria

1. THE Deep_Agent SHALL support StateBackend for ephemeral storage in agent state
2. THE Deep_Agent SHALL support StoreBackend for persistent storage using LangGraph Store
3. THE Deep_Agent SHALL support FilesystemBackend for direct filesystem access
4. THE Deep_Agent SHALL support CompositeBackend for hybrid storage with path-based routing
5. THE Deep_Agent SHALL use StateBackend as the default when no backend is specified

### Requirement 7: Human-in-the-Loop Integration

**User Story:** As a developer, I want to configure human approval for sensitive operations, so that I can ensure appropriate oversight of agent actions.

#### Acceptance Criteria

1. THE Deep_Agent SHALL support interrupt_on configuration for tool-level approval requirements
2. WHEN interrupt_on is configured, THE Deep_Agent SHALL pause execution and wait for human approval
3. THE Deep_Agent SHALL support approve, edit, and reject decisions for interrupted tools
4. THE Deep_Agent SHALL apply HITL configuration to both main agent and subagents
5. THE Deep_Agent SHALL resume execution after receiving human approval

### Requirement 8: Model and Tool Integration

**User Story:** As a developer, I want to integrate custom models and tools with deep agents, so that I can adapt agents to specific domains and requirements.

#### Acceptance Criteria

1. THE Deep_Agent SHALL accept any LangChain-compatible model instance or model string
2. THE Deep_Agent SHALL support custom tools as functions, BaseTool instances, or dictionaries
3. THE Deep_Agent SHALL integrate custom tools with existing middleware tools
4. THE Deep_Agent SHALL support both synchronous and asynchronous tool execution
5. THE Deep_Agent SHALL support MCP (Model Context Protocol) tools via langchain-mcp-adapters

### Requirement 9: Context and Memory Management

**User Story:** As an agent, I want to manage context efficiently across long conversations, so that I can maintain performance while handling complex tasks.

#### Acceptance Criteria

1. THE Deep_Agent SHALL implement automatic summarization when context exceeds 170,000 tokens
2. THE Deep_Agent SHALL keep the last 6 messages when summarizing context
3. THE Deep_Agent SHALL support persistent memory across threads via LangGraph Store
4. THE Deep_Agent SHALL isolate subagent context from main agent context
5. THE Deep_Agent SHALL support Anthropic prompt caching for improved performance

### Requirement 10: Error Handling and Validation

**User Story:** As a developer, I want robust error handling and validation, so that agents fail gracefully and provide meaningful feedback.

#### Acceptance Criteria

1. THE Deep_Agent SHALL validate file paths to prevent directory traversal attacks
2. THE Deep_Agent SHALL return structured error messages for failed operations
3. THE Deep_Agent SHALL handle backend failures gracefully with appropriate fallbacks
4. THE Deep_Agent SHALL validate subagent configurations before creation
5. THE Deep_Agent SHALL provide clear error messages for invalid tool configurations