# claude-papers — Claude Code MCP Plugin

Interact with [Papers](https://wiolett.net) documents directly from Claude Code. Create, edit, organize documents and manage shares — all via natural language.

## Installation

1. Clone and install:
   ```bash
   git clone https://github.com/knownout/claude-papers.git
   cd claude-papers && npm install
   ```

2. Add to your project's `.mcp.json`:
   ```json
   {
     "mcpServers": {
       "papers": {
         "command": "/path/to/claude-papers/node_modules/.bin/tsx",
         "args": ["/path/to/claude-papers/src/index.ts"],
         "env": {
           "PAPERS_HOST": "https://papers.wiolett.net",
           "PAPERS_TOKEN": "tok_..."
         }
       }
     }
   }
   ```

3. Restart Claude Code.

Generate your API token in Papers: **profile dropdown → Settings → API Tokens**.

## Tools

### Documents
| Tool | Description |
|------|-------------|
| `papers_list_documents` | List documents with optional search and pagination |
| `papers_get_document` | Get a document by ID including full content |
| `papers_create_document` | Create a new document |
| `papers_update_document` | Update title, content, visibility, or directory |
| `papers_delete_document` | Delete a document permanently |

### Directories
| Tool | Description |
|------|-------------|
| `papers_list_directories` | List all directories |
| `papers_create_directory` | Create a directory |
| `papers_update_directory` | Rename or move a directory |
| `papers_delete_directory` | Delete a directory |

### Shares
| Tool | Description |
|------|-------------|
| `papers_list_shares` | List shares for a document |
| `papers_create_share` | Share a document with a user |
| `papers_update_share` | Change a share's permission level |
| `papers_delete_share` | Remove a share |

### Attachments
| Tool | Description |
|------|-------------|
| `papers_list_attachments` | List attachments for a document |
| `papers_get_attachment_url` | Get a download URL for an attachment |
| `papers_upload_attachment` | Upload a file (base64-encoded) |
| `papers_delete_attachment` | Delete an attachment |

## Configuration

| Variable | Required | Description |
|----------|----------|-------------|
| `PAPERS_HOST` | Yes | Your Papers instance URL (e.g. `https://papers.wiolett.net`) |
| `PAPERS_TOKEN` | Yes | API token starting with `tok_` |

## Requirements

- Node.js 18+
- A Papers account with API token access
