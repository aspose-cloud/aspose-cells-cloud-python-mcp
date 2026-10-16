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

pip install git+https://github.com/aspose-cloud/aspose-storage-cloud-python-mcp.git git+https://github.com/aspose-cloud/aspose-cells-cloud-python-mcp.git
```

This installs the `aspose-cells-mcp` command into your virtual environment. The first package is the shared file-tool library (upload, download, list, delete) that every Aspose Cloud MCP server depends on; it is not on PyPI, so it is installed from GitHub in the same command.

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
| `cells_get_worksheets_auto_filter` | The tool retrieves the description of the auto‑filter configuration for a specified worksheet within a workbook stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_auto_filter_date_filter` | The tool deletes a date filter that has been applied to a specific column in a worksheet of a given Excel workbook stored in Aspose Cells. | no | yes |
| `cells_delete_worksheets_auto_filter_filter` | The tool deletes the auto‑filter applied to a specific column in a given worksheet of an Excel workbook stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_cells` | The tool retrieves descriptions of cells from a specified worksheet in a spreadsheet file stored in Aspose Cloud. | yes | - |
| `cells_create_worksheets_cells` | Sets the value and data type of a specified cell range in a worksheet of an existing Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_cells_style` | This tool retrieves the full style description of a specific cell in a worksheet stored in Aspose Cloud. | yes | - |
| `cells_create_worksheets_cells_style` | The tool updates the visual style of a single cell identified by its A1‑style address in a specified worksheet of an Excel workbook stored in Aspose Cloud storage. | no | yes |
| `cells_get_worksheets_cells_htmlstring` | The tool retrieves the HTML representation of a specific cell—including its data and formatting—from an Excel workbook stored in Aspose Cloud. | yes | - |
| `cells_get_worksheets_cells_columns` | The tool fetches detailed metadata for the columns of a specified worksheet in a workbook stored in Aspose Cells Cloud. | yes | - |
| `cells_update_worksheets_cells_columns` | The tool inserts a specified number of new columns into a given worksheet of an Excel file at the indicated column index. | no | yes |
| `cells_delete_worksheets_cells_columns` | The tool deletes a specified number of consecutive columns starting at a given column index from a worksheet in an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_cells_rows` | The tool returns the list of row descriptions for a given worksheet in an Excel file stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_cells_rows` | Deletes a contiguous block of rows from a specified worksheet in an Excel file stored on Aspose Cloud. | no | yes |
| `cells_update_worksheets_cells_rows` | Inserts a new row at the specified index into a worksheet of an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_charts` | Retrieves the list of chart descriptions defined in a specific worksheet of an Excel file stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_charts` | The tool deletes all chart objects from a specified worksheet in an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_charts_legend` | Retrieves the legend description for a specified chart within a worksheet of an Excel file stored in Aspose Cloud. | yes | - |
| `cells_create_worksheets_charts_legend` | Updates the legend of a specified chart in a worksheet of an Excel file stored in Aspose Cloud. | no | yes |
| `cells_delete_worksheets_charts_legend` | Deletes the legend from a specified chart in a worksheet of an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_charts_title` | This tool retrieves the title description of a specified chart in a worksheet of an Excel file stored in Aspose Cloud. | yes | - |
| `cells_create_worksheets_charts_title` | Updates the title of a specific chart in a worksheet file stored in Aspose Cloud. | no | yes |
| `cells_delete_worksheets_charts_title` | Deletes the title of the specified chart in a worksheet of a spreadsheet file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_charts_seriesaxis` | The tool retrieves the series‑axis description of a specific chart in an Excel workbook. | yes | - |
| `cells_get_worksheets_charts_categoryaxis` | Retrieves the category axis information for a specified chart in a worksheet of an Excel file stored in Aspose Cloud. | yes | - |
| `cells_create_worksheets_charts_categoryaxis` | Updates the category axis of a specific chart in an Excel worksheet stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_charts_valueaxis` | The tool retrieves the configuration of the value axis for a specific chart stored in an Excel workbook on Aspose Cloud. | yes | - |
| `cells_create_worksheets_charts_valueaxis` | The tool updates the value‑axis configuration of a specific chart in an Excel workbook stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_conditional_formattings` | The tool retrieves a list of all conditional formatting definitions applied to a specified worksheet in an Excel file stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_conditional_formattings` | Removes all conditional formatting rules from a specified worksheet in a given Excel file stored in Aspose Cloud. | no | yes |
| `cells_update_worksheets_conditional_formattings` | The tool adds a conditional formatting rule to a specified worksheet in an existing workbook file. | no | yes |
| `cells_delete_worksheets_conditional_formattings_area` | Deletes a specified cell area from the conditional formatting rules of a worksheet in an Excel file stored in Aspose Cloud. | no | yes |
| `cells_download_workbook` | This tool downloads a workbook from Aspose.Cells storage, optionally converting it to a selected format and applying various export options. | yes | - |
| `cells_create_workbook` | Creates a new Excel workbook in Aspose.Cells cloud storage, optionally using a template and/or smart‑marker data, with options to control overwriting and the target fold… | no | yes |
| `cells_get_worksheets_hyperlinks` | Retrieves the list of hyperlink definitions present in a specified worksheet of an Excel file stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_hyperlinks` | Deletes all hyperlinks from a specified worksheet in an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_listobjects` | Gets the metadata descriptions of all ListObjects (tables) defined in a specified worksheet of an Excel file stored on Aspose Cloud. | yes | - |
| `cells_delete_worksheets_listobjects` | Deletes all ListObjects from the specified worksheet in the given Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_oleobjects` | The tool retrieves the collection of OLE object descriptions from a specified worksheet within a workbook stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_oleobjects` | Deletes all OLE objects from a specified worksheet in an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_verticalpagebreaks` | This tool retrieves the descriptions of all vertical page breaks in a specified worksheet of a workbook stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_verticalpagebreaks` | Deletes all vertical page breaks from the specified worksheet in a workbook, optionally removing only the break at a given column index. | no | yes |
| `cells_get_worksheets_horizontalpagebreaks` | Retrieves the list of horizontal page break definitions for a specified worksheet in a given workbook. | yes | - |
| `cells_delete_worksheets_horizontalpagebreaks` | Deletes horizontal page breaks from a specified worksheet in a workbook, optionally removing only the break at a given zero‑based row index. | no | yes |
| `cells_get_worksheets_pagesetup` | The tool retrieves the page‑setup configuration for a specific worksheet in a given Excel file stored in Aspose Cloud. | yes | - |
| `cells_create_worksheets_pagesetup` | Updates the page‑setup settings of a specified worksheet in an existing Excel file stored in Aspose Cloud. | no | yes |
| `cells_delete_worksheets_pagesetup_clearheaderfooter` | Deletes any existing header and footer from the specified worksheet in an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_pagesetup_header` | The tool retrieves the page‑header configuration for a specified worksheet in a spreadsheet file stored in Aspose Cloud. | yes | - |
| `cells_create_worksheets_pagesetup_header` | This tool updates the header of a specific worksheet in an Excel file stored on Aspose Cloud. | no | yes |
| `cells_get_worksheets_pagesetup_footer` | The tool retrieves the page footer configuration for a specified worksheet in a given Excel file stored in Aspose Cloud. | yes | - |
| `cells_create_worksheets_pagesetup_footer` | Updates the footer of a specific worksheet in an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_pictures` | The tool returns metadata describing each picture embedded in a specified worksheet of an Excel file stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_pictures` | Deletes all picture objects from the specified worksheet in a workbook stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_pivottables` | Retrieves the list of pivot tables defined in a specified worksheet of an Aspose.Cells workbook. | yes | - |
| `cells_delete_worksheets_pivottables` | Deletes all pivot tables from the specified worksheet of the given Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_pivottables_pivot_field` | The tool retrieves the description of a specific pivot field from a PivotTable in an Excel file stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_pivottables_pivot_field` | Deletes a specified pivot field from a pivot table in an Excel workbook stored in Aspose.Cells Cloud. | no | yes |
| `cells_get_workbook_properties` | The tool fetches the collection of document properties defined for a given Excel workbook. | yes | - |
| `cells_delete_workbook_properties` | Deletes all custom document properties and resets built‑in document properties for the specified workbook. | no | yes |
| `cells_get_documentproperties` | Retrieves the value and details of a specific document property from an Excel workbook stored in Aspose Cloud. | yes | - |
| `cells_delete_encryption` | The tool removes the encryption settings from a specified Excel workbook stored in Aspose Cloud, thereby decrypting the file. | no | yes |
| `cells_delete_protection` | The tool removes password‑based protection from a specified Excel workbook stored in Aspose Cloud. | no | yes |
| `cells_delete_write_protection` | This tool removes write protection from a specified Excel workbook so it can be edited. | no | yes |
| `cells_get_worksheets_ranges_value` | Retrieves the values of the cells that lie within a specified range of a worksheet in an Excel file stored in Aspose Cloud. | yes | - |
| `cells_create_worksheets_ranges_value` | Assigns a specified value to every cell within a defined range of a worksheet in an Excel workbook, optionally converting the value’s data type and resetting the cells’… | no | yes |
| `cells_delete_worksheets_ranges` | Deletes a specified range of cells from a worksheet in a workbook and shifts the remaining cells according to the chosen shift option. | no | yes |
| `cells_get_worksheets_shapes` | Retrieves the list of shape descriptions from a specified worksheet in an Excel file stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_shapes` | Deletes all shapes from a specified worksheet in an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_sparkline_groups` | Fetches the metadata descriptions of all sparkline groups defined in a specified worksheet of an Excel file stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_sparkline_groups` | Deletes all sparkline groups from a specified worksheet in an Excel file. | no | yes |
| `cells_get_defaultstyle` | The tool retrieves the default style description of a specified Excel workbook stored in Aspose Cloud. | yes | - |
| `cells_get_text_items` | This tool calls the Aspose Cells API to retrieve every text item contained in a specified workbook file. | yes | - |
| `cells_get_names` | The tool retrieves the list of named ranges defined in a specified Excel workbook stored in Aspose Cloud. | yes | - |
| `cells_delete_names` | Deletes all named ranges from the specified workbook file. | no | yes |
| `cells_get_names_value` | This tool retrieves the current value stored in a specified named range of a workbook. | yes | - |
| `cells_merge_workbooks` | The tool merges a specified source workbook into a target workbook stored in Aspose.Cells cloud storage. | no | yes |
| `cells_replace_text` | The tool replaces every occurrence of a specified text string with a new text string throughout a workbook file stored in Aspose.Cells Cloud. | no | yes |
| `cells_split_workbook` | The tool splits a workbook file into separate files according to a chosen output format, optionally limiting the worksheet range and controlling image resolution or nami… | no | yes |
| `cells_get_settings` | Retrieves the workbook settings for a specified Excel file stored in Aspose Cloud storage. | yes | - |
| `cells_create_settings` | Updates the workbook settings of a specified Excel file stored in Aspose Cloud. | no | yes |
| `cells_delete_background` | The tool deletes any background image set in a workbook file stored in Aspose.Cells Cloud. | no | yes |
| `cells_get_pagecount` | Retrieves the total number of printable pages in a specified Excel workbook stored in Aspose Cloud. | yes | - |
| `cells_get_allstyles` | Retrieves all styles defined in the specified Excel workbook. | yes | - |
| `cells_list_worksheets` | Retrieves the list and basic metadata of all worksheets contained in a specified Excel workbook. | yes | - |
| `cells_delete_worksheets` | Deletes all worksheets in the specified workbook that satisfy the provided matching conditions. | no | yes |
| `cells_get_worksheets` | The tool retrieves a specific worksheet from a workbook and returns its contents exported in the requested file format. | yes | - |
| `cells_update_worksheets` | Adds a new worksheet with a specified name to an existing workbook file. | no | yes |
| `cells_delete_worksheets_protection` | Removes protection from a specified worksheet in an existing Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_text_items` | The tool retrieves every text item (such as cell values, comments, and other textual objects) from a specified worksheet in a given workbook stored in Aspose Cloud. | yes | - |
| `cells_get_worksheets_comments` | Retrieves all comments defined in a specified worksheet of a spreadsheet file stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_comments` | The tool removes every comment from a given worksheet in the specified Excel file stored in Aspose Cloud. | no | yes |
| `cells_update_worksheets_comments` | Adds a comment to a specified cell in a worksheet of an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_merged_cells` | Retrieves the list of merged cell ranges defined in a specified worksheet of a workbook stored in Aspose Cloud. | yes | - |
| `cells_get_worksheets_formula_result` | This tool evaluates a specified Excel formula in a given worksheet of a workbook stored in Aspose Cloud and returns the calculated result. | yes | - |
| `cells_delete_worksheets_background` | Removes the background image from a specific worksheet in an Excel file stored in Aspose Cloud. | no | yes |
| `cells_delete_worksheets_freezepanes` | Unfreeze the specified rows and columns in a worksheet of an Excel file stored in Aspose Cloud. | no | yes |
| `cells_get_worksheets_ranges` | Fetches the collection of named range descriptions from all worksheets in the specified Excel file. | yes | - |
| `cells_get_worksheets_pagecount` | Retrieves the total number of printable pages in a specified worksheet of an Excel file stored in Aspose Cloud. | yes | - |
| `cells_get_worksheets_validations` | The tool retrieves the collection of validation descriptions defined in a specified worksheet of an Excel workbook stored in Aspose Cloud. | yes | - |
| `cells_delete_worksheets_validations` | Deletes all data validation rules from a specified worksheet in a given Excel workbook stored in Aspose Cloud. | no | yes |
| `cells_get_storage_file` | Downloads a file from Aspose Cells cloud storage and returns its binary data. | yes | - |
| `cells_delete_storage_file` | Deletes a file from Aspose.Cells cloud storage. | no | yes |
| `cells_update_storage_file_copy` | Copies a file from one location in Aspose Cloud storage to another location. | no | yes |
| `cells_update_storage_file_move` | The tool moves an existing file from one location to another within Aspose Cells Cloud storage. | no | yes |
| `cells_get_storage_folder` | Gets a list of all files and sub‑folders contained in a given folder in Aspose Cloud storage. | yes | - |
| `cells_update_storage_folder` | Creates a new folder at the specified path in Aspose Cells Cloud storage. | no | yes |
| `cells_delete_storage_folder` | Deletes a specified folder from Aspose Cells Cloud storage, optionally removing all contained subfolders and files. | no | yes |
| `cells_update_storage_folder_copy` | Copies a folder from a specified source location to a specified destination location in Aspose Cells cloud storage. | no | yes |
| `cells_update_storage_folder_move` | Moves a folder from a specified source path to a specified destination path in Aspose Cells cloud storage. | no | yes |
| `cells_get_storage_exist` | The tool checks whether a storage container with a given name exists in Aspose.Cells Cloud. | yes | - |
| `cells_get_storage_disc` | Retrieves the current disk usage statistics for a specified Aspose.Cells storage. | yes | - |
| `cells_get_storage_version` | The tool retrieves the list of version metadata for a specified file stored in Aspose.Cells cloud storage. | yes | - |

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
