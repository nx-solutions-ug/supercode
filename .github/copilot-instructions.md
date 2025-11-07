# SuperCode Framework - AI Agent Instructions

## Architecture Overview

SuperCode is a **multi-agent AI framework** that extends OpenCode with specialized agents, commands, MCP integrations, and behavioral flags. The architecture follows a three-tier system:

1. **Command Layer** (`/command/*.md`): User-facing slash commands (`/initialize`, `/implement`, `/load`) that trigger workflows
2. **Agent Layer** (`/agent/*.md`): 19 specialized agents with defined roles, tools, and boundaries (orchestrator, backend-engineer, qa-engineer, etc.)
3. **MCP Integration Layer** (`/MCP_*.md`): 11 external tool servers (Knowledge Graph, Memory Bank, Sequential, Context7, etc.)

**Critical Insight**: Agents are **instruction files** loaded by OpenCode, not running processes. Commands delegate work to agents via the Task tool. The orchestrator agent coordinates complex multi-domain tasks.

## Essential Workflows

### Installation & Initialization
```bash
# Install SuperCode into user's project
npm run supercode:init /path/to/project

# What this does:
# 1. Runs install.sh → copies agent/*.md and command/*.md to ~/.config/opencode/
# 2. Copies templates/opencode.jsonc.template to project root
# 3. Executes /initialize to build knowledge graph and memory bank
```

**Never run `npm run build` in root** - this is a meta-framework, not a buildable application.

### Context Loading Pattern
```bash
/load  # ALWAYS run first in new conversations to restore project state
```

The `/load` command retrieves:
- Memory Bank files (projectbrief.md, activeContext.md, progress.md) from `.memory-bank/`
- Knowledge Graph entities from `.aim/memory.jsonl`
- Git context (branch, status) and directory structure

**Best Practice**: Orchestrator agent should call `/load` once per session, cache context internally to avoid redundant operations.

### Feature Implementation Flow
```bash
/implement --ultrathink --orchestrate "feature description"
```

Triggers:
1. **Orchestrator** decomposes task into subtasks
2. **Delegates** to specialist agents (backend-engineer, frontend-engineer, qa-engineer)
3. **Sequential MCP** plans multi-step reasoning if `--ultrathink` flag present
4. **Memory Bank** stores architectural decisions for future reference

## Project-Specific Conventions

### Agent Definition Structure
All agents follow `templates/agent.template.md`:
- **Frontmatter**: YAML with agent name, mode (primary/subagent), tools (bash, write, edit), reasoningEffort, temperature
- **Triggers**: 3-5 specific scenarios that activate this agent
- **Boundaries**: Explicit "WILL/WILL NOT" sections prevent scope creep
- **Tool Usage**: Primary tools (1-3) with real MCP integration examples

**Example from orchestrator.md**:
```yaml
---
agent: orchestrator
mode: primary
temperature: 0.7
tools:
  bash: false  # Orchestrator doesn't execute code directly
  write: false # Delegates to specialist agents
---
```

### Command Structure
Commands (`/command/*.md`) follow this pattern:
1. **General Context**: Execute shell commands to gather current state (`!pwd`, `!git status`)
2. **Workflow**: Three-phase structure (Analysis → Action → Finalization)
3. **Examples**: 5-7 real usage patterns with flags
4. **Boundaries**: WILL/WILL NOT sections defining command scope

**Critical**: Commands execute bash commands inline using `!command` syntax for context gathering.

### Behavioral Flags System
Flags modify agent behavior (`FLAGS.md`):
- **Analysis Depth**: `--think` (4K tokens), `--think-hard` (10K), `--ultrathink` (32K)
- **MCP Activation**: `--c7` (Context7), `--seq` (Sequential), `--memory-bank`, `--knowledge-graph`
- **Execution Control**: `--orchestrate`, `--delegate`, `--loop --iterations 5`
- **Token Efficiency**: `--token-efficient` (30-50% reduction via symbols)

**Example**: `/implement --ultrathink --orchestrate --c7` enables maximum reasoning depth + multi-agent coordination + framework docs.

### Memory Architecture (Dual Persistence)
SuperCode uses two complementary memory systems:

**Knowledge Graph** (`.aim/memory.jsonl`):
- Entity-relationship storage (people, projects, components)
- Fast keyword search via `aim_search_nodes()`
- Relations between entities (`aim_create_relations()`)
- Example: "UserAuthModule" entity with observations ["Uses JWT", "Connects to AuthService"]

**Memory Bank** (`.memory-bank/{project}/`):
- Structured markdown files per project
- Standard files: projectbrief.md, activeContext.md, systemPatterns.md, techContext.md, progress.md
- Read via `memory_bank_read({projectName, fileName})`
- Write via `memory_bank_write()` or `memory_bank_update()`

**When to use which**:
- Knowledge Graph: Persistent entities, cross-project patterns, relationship mapping
- Memory Bank: Project documentation, architectural decisions, active work context

## Critical Safety Rules (RULES.md)

1. **NEVER run `npm run build` in root** - no build process exists at framework level
2. **NEVER execute shell commands via Knowledge Graph MCP** - use Desktop Commander MCP instead
3. **ALWAYS terminate dev servers before ending session** - prevent port conflicts
4. **REMOVE developer comments `<!-- ... -->`** from final generated markdown
5. **STORE generated .md in `/opendocs/general`** (or `/opendocs/qa` for QA agent)
6. **USE Conventional Commits format** - all commit messages must follow the format: `type(scope): description`
   - Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `ci`, `build`, `revert`
   - Example: `feat(agent): add database-engineer agent with schema design capabilities`
   - Example: `fix(command): correct /load context retrieval from memory bank`

## Integration Points

### MCP Server Usage Patterns

**Context7** (`--c7`): Framework documentation lookup
```javascript
// When user asks "how to use React hooks?"
mcp_upstash_conte_resolve_library_id({libraryName: "react"})
mcp_upstash_conte_get_library_docs({
  context7CompatibleLibraryID: "/facebook/react",
  topic: "hooks"
})
```

**Sequential** (`--seq`): Multi-step reasoning for complex debugging
```javascript
// For "why is authentication failing?"
sequential_thinking({
  task: "Debug authentication failure",
  steps: ["Check token generation", "Verify token validation", "Test session persistence"]
})
```

**Knowledge Graph**: Store project intelligence
```javascript
aim_create_entities({
  entities: [{
    name: "AuthenticationModule",
    entityType: "component",
    observations: ["JWT-based", "Refresh token rotation", "Redis session storage"]
  }]
})
```

## Development Patterns

### Agent Delegation (Orchestrator Pattern)
```markdown
# In orchestrator.md
Use Task tool to invoke specialist agents:

Task({
  agent: "backend-engineer",
  prompt: "Implement REST API endpoints for user management with JWT authentication. 
          Follow existing patterns in /src/api/. Use TypeScript with Express."
})
```

### Template-Based Generation
When creating new agents/commands, use templates:
- `templates/agent.template.md` for agents
- `templates/command.template.md` for commands
- `templates/MCP_Server.template.md` for MCP documentation

**Validation**: QA agent checks template compliance before approval.

### File Organization
- `/agent/` - Agent definitions (19 agents)
- `/command/` - Command definitions (13 commands)
- `/templates/` - Templates for agent/command/MCP generation
- `/opendocs/general/` - Generated documentation
- `/opendocs/qa/` - QA reports, approvals, standards
- `/landingpage/` - Separate Vite+React project (has own package.json)

**Important**: Landingpage is independent - `cd landingpage && npm run build` for deployment.

## Quick Reference

### First Steps in New Conversation
1. Run `/load` to restore context from Memory Bank + Knowledge Graph
2. Check `!git status` and `!pwd` for current state
3. Review AGENTS.md, RULES.md, PRINCIPLES.md for framework guidelines

### Implementing Complex Features
1. Use `/implement --ultrathink --orchestrate` for multi-domain work
2. Orchestrator decomposes into subtasks → delegates to specialists
3. Store architectural decisions in Memory Bank (`memory_bank_write()`)
4. Create component entities in Knowledge Graph (`aim_create_entities()`)

### Troubleshooting
- Check RULES.md for safety constraints
- Review agent's "Boundaries" section for scope limits
- Use `--seq` flag for systematic debugging with Sequential MCP
- Root cause analyst agent specializes in error diagnosis

### Testing & QA
- QA engineer agent validates all generated content
- Stores reports in `/opendocs/qa/reports/`
- Uses templates from `/opendocs/qa/templates/`
- Checks template compliance, documentation standards, code quality
