# AI-BELARUS — Belarus Business Registry (MCP & Open API)

[![mcptry](https://mcptry.com/badge/by.ai-belarus/registry)](https://mcptry.com/servers/by.ai-belarus/registry)

Public **Model Context Protocol (MCP)** server and **OpenAPI** specification for **[AI-BELARUS (ai-belarus.by)](https://ai-belarus.by)** — a machine-first registry of Belarusian legal entities and sole proprietors backed by official state registries.

> **Этот репозиторий содержит только публичные спецификации и документацию входа MCP/OpenAPI.** Исходный код сервиса и внутренние маршруты здесь не публикуются.

## Epistemic Guarantees / Принципы доказательности

- **Source + raw `sha256` on every fact:** Every statement carries the official holder (`BY:MNS:GRP`, `BY:EGR`, `BY:BANKROT`, `BY:KGK`, `BY:GIAS`), retrieval timestamp (`retrieved_at`), and `sha256` of the raw holder answer so any client can verify facts independently.
- **Three-valued outcomes (`да` | `нет` | `не_установлено`):** Unknown or unavailable registry data is never collapsed into `нет`.
- **Withheld fields explicitly reasoned:** Fields restricted under Belarus Law No. 99-Z on Personal Data Protection (such as sole proprietor details beyond full name when queried by UNP) are returned in `withheld` with the explicit legal/policy reason.
- **No synthetic scores:** No arbitrary trust scores, probabilities, or automated recommendations.

## Read-Only MCP Tools (`https://ai-belarus.by/mcp`)

| Tool | Mode | Description |
|---|---|---|
| `lookup_company` | read-only | Collected public card of a Belarusian entity by 9-digit UNP (`unp`) with source, timestamp, raw `sha256`, and withheld reasons. Instant, no live registry call. |
| `search_companies` | read-only | Search collected Belarusian legal entities by name fragment or 9-digit UNP (`query`, `limit`). Sole proprietors are never returned by name search. |
| `check_counterparty` | read-only | Live check against MNS GRP and linked state registries (`unp`, optional `name` match): status, registration date, address, tax office, bankruptcy (`BY:BANKROT`), procurement exclusion (`BY:GIAS`), KGK inspection plans (`BY:KGK`), procurement history. |
| `entity_timeline` | read-only | Ordered log of canon changes per predicate (`created`, `reaffirmed`, `superseded`, `conflict_detected`) with source registry and timestamps (`unp`, `limit`). |
| `sources_and_coverage` | read-only | Machine-readable trust card of all sources, provenance, declared access/rights status, claim/evidence counts, and grounding invariants (`source_id`). |

## Quick Start

### Claude Code

```bash
claude mcp add --transport http registry https://ai-belarus.by/mcp
```

### Cursor / Claude Desktop / Windsurf (`mcp.json`)

```json
{
  "mcpServers": {
    "ai-belarus": {
      "type": "streamable-http",
      "url": "https://ai-belarus.by/mcp"
    }
  }
}
```

### Direct JSON-RPC (`curl`)

```bash
curl -s -X POST https://ai-belarus.by/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'
```

## Public Machine Endpoints

- **MCP endpoint (streamable-http):** `https://ai-belarus.by/mcp`
- **MCP Registry entry:** [`server.json`](server.json) (`by.ai-belarus/registry`)
- **OpenAPI specification (public routes):** [`openapi.json`](openapi.json) · live at [`https://ai-belarus.by/openapi-gpt.json`](https://ai-belarus.by/openapi-gpt.json)
- **LLM index:** [`llms.txt`](llms.txt) · live at [`https://ai-belarus.by/llms.txt`](https://ai-belarus.by/llms.txt)
- **Agent guide:** [`https://ai-belarus.by/agents`](https://ai-belarus.by/agents)
- **Service status & source freshness:** [`https://ai-belarus.by/status`](https://ai-belarus.by/status)
- **Data subject rights (Law 99-Z):** [`https://ai-belarus.by/rights`](https://ai-belarus.by/rights)

## Operator & Legal Notice

Operated by **Sergei Tabalevich** (Self-Employed, Professional Income Tax Payer, UNP `AC0463536`, Minsk, Belarus).
Re-distribution of facts obtained through the service must preserve official holder and distributor attribution (`attribution`, Belarus Law No. 455-Z Art. 20) and comply with Belarus Law No. 99-Z on Personal Data Protection. State registry datasets remain subject to the terms of their respective state holders.
Public offer (NPD, green-zone services): [ai-belarus.by/offer](https://ai-belarus.by/offer).
