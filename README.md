# Aspose.Cells Cloud MCP Server

Spreadsheet processing — convert, create, merge, split, replace text, and read workbook properties/worksheets.

An [MCP](https://modelcontextprotocol.io) server exposing Aspose.Cells Cloud's REST API as typed,
agent-callable tools. Also bundles Aspose Storage Cloud's core file operations
(`storage_upload_file`/`storage_download_file`/`storage_list_files`/`storage_delete_file`), so a
client connected to only this server can complete a full upload -> process -> download workflow with
no second server connection.

Handles: XLSX, XLS, CSV, ODS.

---

## Requirements

- Python 3.11 or later
- An [Aspose Cloud](https://dashboard.aspose.cloud/) account (free evaluation tier available) - you'll
  need a **Client ID** and **Client Secret** from your dashboard's Applications page
- An MCP-compatible AI client (Claude Desktop, Claude Code, VS Code, Cursor, Cline, Windsurf, etc.)

---

## Setup

### 1. Create a virtual environment and install

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS / Linux:
source .venv/bin/activate

pip install git+https://github.com/aspose-cloud/aspose-cells-cloud-python-mcp.git
```

This installs the `aspose-cells-mcp` command into your virtual environment.

### 2. Configure your AI client

Two environment variables are required - both come from your Aspose Cloud dashboard's Applications page:

| Variable | Value |
|---|---|
| `ASPOSE_CLIENT_ID` | Your application's Client ID |
| `ASPOSE_CLIENT_SECRET` | Your application's Client Secret |

Credentials are never passed as a tool parameter - the server resolves them once at launch from
these environment variables, exchanges them for a short-lived OAuth2 token, and caches/refreshes it
transparently.

#### Claude Desktop

Config file location:

| Platform | Path |
|---|---|
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |

```json
{
  "mcpServers": {
    "cells": {
      "command": "C:\\path\\to\\.venv\\Scripts\\aspose-cells-mcp.exe",
      "env": {
        "ASPOSE_CLIENT_ID": "your-client-id",
        "ASPOSE_CLIENT_SECRET": "your-client-secret"
      }
    }
  }
}
```

On macOS/Linux, use `/path/to/.venv/bin/aspose-cells-mcp` instead. Fully quit and restart Claude Desktop after
editing.

#### VS Code (`.vscode/mcp.json`), Cursor (`~/.cursor/mcp.json`), Cline, Windsurf

Same shape, under a `"servers"` key instead of `"mcpServers"` for VS Code:

```json
{
  "servers": {
    "cells": {
      "type": "stdio",
      "command": "/path/to/.venv/bin/aspose-cells-mcp",
      "env": {
        "ASPOSE_CLIENT_ID": "your-client-id",
        "ASPOSE_CLIENT_SECRET": "your-client-secret"
      }
    }
  }
}
```

---

## Available tools

| Tool | What it does | Read-only | Dry-run |
|---|---|---|---|
| `cells_download_workbook` | Download an existing Cloud-stored Excel workbook in a given format | yes | - |
| `cells_create_workbook` | Create a new Excel workbook in Aspose Cloud Storage | no | yes |
| `cells_get_workbook_properties` | Get an Excel workbook's document properties | yes | - |
| `cells_delete_workbook_properties` | Delete all of an Excel workbook's document properties | no | yes |
| `cells_merge_workbooks` | Merge another Excel workbook into an existing one | no | yes |
| `cells_replace_text` | Replace text throughout an Excel workbook | no | yes |
| `cells_split_workbook` | Split an Excel workbook into separate worksheet files | no | yes |
| `cells_list_worksheets` | List an Excel workbook's worksheets | yes | - |

Every mutating tool marked "yes" under **Dry-run** accepts a `dry_run=true` parameter to preview
the change without applying it.

Tool errors use a fixed taxonomy (bad input / auth failure / server error / rate limited), returned
as structured MCP tool errors - never a silent failure or a raw exception message.

---

## Part of the Aspose Cloud MCP family

One MCP server per Aspose Cloud product, published under [github.com/aspose-cloud](https://github.com/aspose-cloud).
This server depends on [`aspose-storage-core-mcp`](https://github.com/aspose-cloud/aspose-storage-cloud-python-mcp)
for its bundled storage tools — that repo is a shared library, not a standalone server (there's no
real Aspose Cloud API route for storage on its own; every real storage call goes through some
product's own gateway, this one included).

---

## License

MIT (see [`LICENSE`](LICENSE)) — covers only this repository's own MCP wrapper/integration code.
It does **not** cover, and grants no rights to, the Aspose Cloud product or API themselves, which
remain governed entirely by [Aspose's own product and usage terms](https://purchase.aspose.cloud/policies).
A valid Aspose Cloud account and subscription/credentials are required to actually call the API,
regardless of this code's license.
