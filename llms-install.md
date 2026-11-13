# Installing Ask the Clock (for AI agents and MCP clients)

Ask the Clock is a remote MCP server. Nothing needs to be downloaded, built or run locally.

## Settings

- Name: `Ask the Clock`
- Transport: Streamable HTTP
- URL: `https://ctrlworx.com/wp-json/ctrlworx/v1/ask-the-clock`
- Authentication: none (no API key, no OAuth)
- MCP Registry name: `com.ctrlworx/ask-the-clock`

## Client configuration (JSON)

For clients that read an `mcpServers` block:

```json
{
  "mcpServers": {
    "ask-the-clock": {
      "type": "http",
      "url": "https://ctrlworx.com/wp-json/ctrlworx/v1/ask-the-clock"
    }
  }
}
```

For clients that start local (stdio) servers and nothing else, use a remote bridge such as `mcp-remote`:

```json
{
  "mcpServers": {
    "ask-the-clock": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://ctrlworx.com/wp-json/ctrlworx/v1/ask-the-clock"]
    }
  }
}
```

## Check that it works

1. List tools. You should see `lookup_company` and `clock_headline`.
2. Call `clock_headline` with no arguments. The reply starts with the number of public companies on the Clock and gives its as-of date, the October report figures and source links.
3. Call `lookup_company` with `{"query": "<a ticker>"}`. The reply gives the company's status and the link to its SEC filing.

## Notes for agents

- Both tools read data and never change it, so they are safe to call without asking the user to confirm.
- Quote the as-of date with any figure; the live counts change every day.
- If `lookup_company` returns candidates, ask the user which company they mean rather than picking one.
- A material weakness is a statement about internal control, not a finding of wrongdoing; don't describe it as anything else.
- Busy callers are rate limited for a short time; wait and retry.

More: https://ctrlworx.com/ask-the-clock/
