# Product Overview

DeepAgents is a Python package that implements "deep agents" - LLM-powered agents that go beyond simple tool-calling loops to handle complex, multi-step tasks through planning, context management, and hierarchical task delegation.

## Core Concept

Unlike shallow agents that just call tools in a loop, DeepAgents implements four key capabilities:
- **Planning tool** - Agents can break down complex tasks into discrete steps and track progress
- **Sub agents** - Spawn specialized agents for context isolation and focused subtasks  
- **File system** - Offload large context to memory, preventing context window overflow
- **Detailed prompts** - Heavily inspired by Claude Code's system prompt architecture

## Key Features

- **Planning & Task Decomposition** via built-in `write_todos` tool
- **Context Management** through filesystem tools (ls, read_file, write_file, edit_file, glob, grep)
- **Subagent Spawning** with the `task` tool for specialized work
- **Long-term Memory** using LangGraph's Store for persistent memory across threads
- **Modular Middleware Architecture** for composable functionality
- **Human-in-the-Loop** support for sensitive operations

## Target Use Cases

- Complex research tasks requiring multi-step information gathering
- Code analysis and development workflows
- Long-running tasks that benefit from planning and progress tracking
- Applications requiring context isolation between different subtasks