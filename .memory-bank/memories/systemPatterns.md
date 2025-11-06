# System Patterns: SuperCode Framework

## System Architecture
- **Dual MCP Foundation**: Knowledge Graph for entity relationships + Memory Bank for project context
- **Agent-Based Architecture**: 19 specialized agents with distinct responsibilities
- **Command System**: 13 reusable commands for common workflows
- **Template System**: Standardized patterns for agents, commands, and MCP servers
- **React/TypeScript Frontend**: Vite-based build system with modern UI components

## Key Technical Decisions
- **OpenCode Plugin Architecture**: Framework extends OpenCode capabilities
- **TypeScript Strict Mode**: Full type safety with no implicit any
- **Functional React Components**: Hooks-based architecture with modern patterns
- **Path Aliases**: @/* for clean imports and better code organization
- **ESLint Zero Warnings Policy**: Strict linting with automatic fixes

## Design Patterns in Use
- **Command Pattern**: For reusable workflow implementations
- **Factory Pattern**: For agent creation and configuration
- **Observer Pattern**: For agent orchestration and event handling
- **Strategy Pattern**: For behavioral flag implementations
- **Template Method**: For standardized agent and command structures

## Component Relationships
- **Agent/Command Coordination**: Orchestrator manages complex task distribution
- **MCP Server Integration**: Unified interface for external tool integration
- **Memory System Interaction**: Dual memory management with deduplication
- **Frontend/Backend Separation**: Clear boundaries between UI and logic

## Critical Implementation Paths
- **Project Initialization**: /initialize command establishes dual MCP foundation
- **Context Loading**: /load retrieves project state from both memory systems
- **Agent Orchestration**: Multi-agent coordination through behavioral flags
- **Build System**: Vite optimization for production deployment