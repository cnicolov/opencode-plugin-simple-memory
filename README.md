# Simple Memory Plugin for OpenCode

[![npm version](https://img.shields.io/npm/v/@knikolov/opencode-plugin-simple-memory)](https://www.npmjs.com/package/@knikolov/opencode-plugin-simple-memory)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A persistent memory plugin for [OpenCode](https://opencode.ai) that enables the AI assistant to remember context across sessions.

## Setup

1. Add the plugin to your [OpenCode config](https://opencode.ai/docs/config/):

   ```json
   {
     "$schema": "https://opencode.ai/config.json",
     "plugin": ["@knikolov/opencode-plugin-simple-memory"]
   }
   ```

2. Start using memory commands in your conversations.

Memories are stored in `.opencode/memory/` as daily logfmt files.

## Updating

> [!WARNING]
> OpenCode does NOT auto-update plugins.

To get the latest version, clear the cached plugin and let OpenCode reinstall it:

```bash
# Remove the plugin from cache
rm -rf ~/.cache/opencode/node_modules/@knikolov/opencode-plugin-simple-memory

# Run OpenCode to trigger reinstall
opencode
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

## Example Usage

```
User: Remember that my name is Kris

AI: [calls memory_remember with type="context", scope="user", 
     content="User's name is Kris"]
    
    I've stored that your name is Kris.

User: My preferred programming language is Erlang

AI: [calls memory_remember with type="preference", scope="user", 
     content="Preferred programming language is Erlang"]
    
    Got it! I've noted that your preferred programming language is Erlang.

User: What do you know about me?

AI: [calls memory_recall with scope="user"]
    
    Here's what I know about you:
    - Your name is Kris
    - Your preferred programming language is Erlang

User: Actually, I prefer Elixir now.

AI: [calls memory_update with type="preference", scope="user", 
     content="Preferred programming language is Elixir"]
    
    Updated your language preference to Elixir.

User: Forget my language preference

AI: [calls memory_forget with type="preference", scope="user", 
     reason="User requested removal"]
    
    Done. I've removed your language preference from memory.
```

## Local Development

Clone the repository and install dependencies:

```bash
git clone https://github.com/cnicolov/opencode-plugin-simple-memory.git
cd opencode-plugin-simple-memory
bun install
```

Point your OpenCode config to the local checkout via a `file://` URL:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": ["file:///absolute/path/to/opencode-plugin-simple-memory"]
}
```

Replace `/absolute/path/to/opencode-plugin-simple-memory` with your actual path.
