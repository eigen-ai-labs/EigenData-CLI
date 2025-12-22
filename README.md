# EigenData CLI

Natural language-driven data generation platform for creating high-quality conversational training data.

## Installation

```bash
npm install -g @eigenai/eigendata-cli
```

**Requirements:** Node.js >= 18, Linux (x64/arm64)

After installation, run:

```bash
eigendata
```

## Quick Start

1. **Get an API key** at [https://train.eigenai.com](https://train.eigenai.com)
2. Run `eigendata` and enter your API key when prompted
3. Start generating data using natural language commands

```
eigendata> Generate 10 multi-turn customer support dialogs for laptop troubleshooting
```

## Features

| Command | Description |
|---------|-------------|
| **generate** | Create conversational data from scratch |
| **schema-refine** | Improve function/tool schemas |
| **modify** | Transform or enhance existing datasets |
| **analyze** | Validate data quality and identify issues |

## Usage

EigenData uses a natural language interface. Simply describe what you want:

```
eigendata> Generate function-calling dialogs for an airline booking system
eigendata> Refine the schema for my API tools
eigendata> Modify my dataset to include more edge cases
eigendata> Analyze the quality of outputs/generated_data_xxxx.jsonl
```

## Slash Commands

| Command | Description |
|---------|-------------|
| `/configure` | Update settings (API key, workspace, MCP server, etc.) |
| `/tutorial` | Toggle tutorial hints (`/tutorial false` to disable) |
| `/view` | Launch web-based data viewer |
| `/version` | Display CLI version |

## Directory Structure

### Settings Directory (`~/.eigendata/`)

```
~/.eigendata/
└── settings.yaml    # API key, workspace path, MCP server URL, etc.
```

### Workspace Directory (`~/eigendataDB/`)

```
~/eigendataDB/
├── config/          # User configuration overrides
├── input/           # Input data files (for modify/analyze)
├── outputs/         # Generated output data (.jsonl files)
└── logs/            # Session logs and execution history
```

## Configuration

Settings are stored in `~/.eigendata/settings.yaml`:

| Key | Description |
|-----|-------------|
| `api_key` | EigenData API key |
| `eigendata_db_path` | Workspace directory path |
| `mcp_server_url` | MCP server URL for tool environments |
| `schema_file_path` | Local schema file path (JSON/YAML) |
| `tutorial_enabled` | Show/hide tutorial hints |

Update settings interactively:

```
eigendata> /configure
```

## Schema Sources

EigenData requires a function schema for data generation. Provide one via:

1. **MCP Server** (recommended): Connect to an MCP-compatible tool server
2. **Local File**: JSON or YAML schema file

Configure via `/configure` or when prompted during task execution.

## Output Formats

- **Generated data**: JSONL files in `~/eigendataDB/outputs/`
- **Analysis reports**: JSON files with quality metrics
- **Refined schemas**: JSON/YAML files with diff comparison
