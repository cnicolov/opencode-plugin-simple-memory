# OpenCode Simple Memory Plugin

[![npm version](https://img.shields.io/npm/v/@knikolov/opencode-plugin-simple-memory.svg)](https://www.npmjs.com/package/@knikolov/opencode-plugin-simple-memory)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A persistent memory plugin for [OpenCode](https://opencode.ai) that enables the AI assistant to remember context across sessions.

## Features

- **Persistent Storage**: Memories stored in logfmt format, organized by date
- **Memory Types**: Support for decisions, learnings, preferences, blockers, context, and patterns
- **Scoped Organization**: Organize memories by scope (e.g., user, project, api, auth)
- **Search & Filtering**: Query memories by scope, type, or free-text search
- **Audit Trail**: Deletions are logged for accountability

## Installation

Add to your `~/.config/opencode/opencode.json`:

```json
{
  "plugin": [
    "@knikolov/opencode-plugin-simple-memory"
  ]
}
```

## Tools

The plugin provides five tools:

| Tool | Description |
|------|-------------|
| `memory_remember` | Store a new memory |
| `memory_recall` | Retrieve memories by scope, type, or search query |
| `memory_update` | Update an existing memory |
| `memory_forget` | Delete a memory (with audit logging) |
| `memory_list` | List all scopes and types for discovery |

## Memory Types

- `decision` - Architectural or design decisions
- `learning` - Things learned during development
- `preference` - User or project preferences
- `blocker` - Current blockers or issues
- `context` - General context information
- `pattern` - Recurring patterns or conventions

## Storage

Memories are stored in `.opencode/memory/` as daily logfmt files:

```
.opencode/memory/
  2025-12-15.logfmt
  deletions.logfmt    # audit log
```

## Example Chat

```
User: What do you know about me?

AI: I don't have any stored information about you yet. Let me check if there are 
    any memories saved in the system.
    
    [calls memory_list]
    
    Total memories: 2
    Scopes:
      user: 2 (context)
    Types:
      context: 2
    
    [calls memory_recall with scope="user"]
    
    Found 2 memories:
    [2025-12-15] context/user: User's name is Kris
    [2025-12-15] context/user: Passionate about Software Engineering
    
    Based on the stored memories, here's what I know about you:
    1. Name: Kris
    2. Interest: You're passionate about Software Engineering

User: My preferred programming language is Erlang

AI: [calls memory_remember with type="preference", scope="user", 
     content="Preferred programming language is Erlang"]
    
    Got it, Kris. I've stored that your preferred programming language is Erlang.
    A solid choice for concurrent, fault-tolerant systems!

User: What do you know about me now?

AI: [calls memory_recall with scope="user"]
    
    Found 3 memories:
    [2025-12-15] context/user: User's name is Kris
    [2025-12-15] context/user: Passionate about Software Engineering
    [2025-12-15] preference/user: Preferred programming language is Erlang
    
    Here's everything I know about you:
    1. Name: Kris
    2. Passionate about Software Engineering
    3. Preferred programming language: Erlang

User: Actually, I changed my mind. I prefer Elixir now.

AI: [calls memory_update with type="preference", scope="user", 
     content="Preferred programming language is Elixir"]
    
    Updated! Your language preference is now set to Elixir. Makes sense - you 
    get the Erlang VM benefits with a more modern syntax.

User: Forget my language preference entirely

AI: [calls memory_forget with type="preference", scope="user", 
     reason="User requested removal of language preference"]
    
    Done. I've removed your language preference from memory. The deletion has 
    been logged for audit purposes.
```

## License

MIT
