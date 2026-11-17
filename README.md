# ServiceBox MCP server

Connect ServiceBox to ChatGPT, Claude and any AI agent through Cotlet.dev. Work in your own signed-in ServiceBox user with 21 hosted workflows for work orders, job costs, equipment, quotes, invoices and customer email. Write actions ask for approval by default.

## What you can ask

> List my unscheduled work orders.

> Compare actual costs with the quote for a work order.

> Show invoices nobody has opened.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its ServiceBox tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 servicebox_agent.py serve
```

Or with Docker:

```bash
docker build -t servicebox-mcp .
docker run -i --rm -e COTLET_API_KEY servicebox-mcp
```

Tools: `servicebox_guide`, `servicebox_status`, `servicebox_open`, `servicebox_read`, `servicebox_click`, `servicebox_type`, `servicebox_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 servicebox_agent.py connect   # ready config for ChatGPT, Claude, Claude Code and Cursor
python3 servicebox_agent.py status    # is ServiceBox ready in your Cotlet.dev account
python3 servicebox_agent.py guide     # ServiceBox screens and sign-in link
python3 servicebox_agent.py read      # open ServiceBox, read the page, close the browser
python3 servicebox_agent.py tools     # list Cotlet.dev tools
python3 servicebox_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from ServiceBox users

**Which ServiceBox user should I connect?**

Sign in at https://jb.jobboxsoft.com/servicebox in your Cotlet Cloud Browser. An administrator sees every work order; a basic user sees only assigned work orders.

**Which actions ask for approval?**

The hosted integration has 8 read tools and 13 write tools. Every write asks for approval by default.

**Is Cotlet affiliated with ServiceBox?**

No. Cotlet.dev is independent and not affiliated with ServiceBox or Jobbox Software.

## Learn more

- [ServiceBox integration and hosted tools](https://cotlet.dev/integrations/servicebox-api-mcp-server)

---

More: https://cotlet.dev/integrations/servicebox-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=servicebox

Request access: https://cotlet.dev/signup?app=servicebox&utm_source=github&utm_medium=readme&utm_campaign=servicebox

## Workflows on the hosted integration

The live integration page documents these 21 operations:

- `list_work_orders`
- `save_work_order`
- `schedule_work_order`
- `attach_checklist`
- `add_inventory_to_work_order`
- `add_service_charge`
- `record_purchase_order`
- `get_job_costing`
- `list_units`
- `manage_units`
- `move_job_site`
- `import_export_records`
- `list_quotes`
- `create_quote`
- `set_quote_financing_terms`
- `list_invoices`
- `get_invoice_statistics`
- `manage_invoices`
- `manage_payments`
- `get_email_log`
- `search_address_book`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
