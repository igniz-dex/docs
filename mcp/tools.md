> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](mcp/tools.md).

# Tools

> **Note:** This is a sample page. The full catalog of MCP tools will be imported here later.

#### About MCP Tools <a href="#about-mcp-tools" id="about-mcp-tools"></a>

Once connected through the [Model Context Protocol](mcp.md), an authorized agent can call a catalog of Igniz tools to read market data, inspect account state, and — when explicitly granted trading authority — place and manage orders.

#### Sample: Tool Entry <a href="#sample-tool-entry" id="sample-tool-entry"></a>

The final page will document each tool with its name, purpose, required authorization tier, parameters, and an example call. The entry below is illustrative only.

| Field | Value |
|---|---|
| Tool | `get_ticker` |
| Category | Market Data (read-only) |
| Authorization | Ask & Analyze |
| Parameters | `symbol` (string) |
| Returns | Latest price and 24h statistics for the symbol |

> The tool name, parameters, and authorization tier above are placeholders. The authoritative tool catalog will replace this page.
