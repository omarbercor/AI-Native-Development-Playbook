# MCP Servers

This directory contains all Model Context Protocol (MCP) servers for this project.
Each MCP lives in its own isolated subdirectory following the convention defined in `.ai/02_mcp_standards.md`.

## Directory Convention

```
mcp/
└── [mcp-name]/          ← kebab-case, descriptive of domain
    ├── index.js          ← MCP server entry point
    ├── package.json      ← Isolated dependencies
    ├── .env.example      ← Public template with variable descriptions
    ├── .env              ← Real secrets (never committed — in .gitignore)
    └── README.md         ← Tool docs, setup, and mcp.json snippet
```

## Adding a New MCP

1. Create a new folder: `mcp/[mcp-name]/`
2. Follow the checklist in `.ai/02_mcp_standards.md §5`
3. Register it in `.ai/mcp.json`
4. Update the table in `.ai/02_mcp_standards.md §8`
5. Update the table in `.ai/active_context.md §Active MCPs`

## Active Servers

| Folder | Domain | Status |
|---|---|---|
| *(empty — add servers as the project requires)* | | |

> See `.ai/mcp.json` for the runtime configuration of each server.
