---
name: "vibe"
displayName: "Vibe MCP"
description: "Integração com Vibe MCP para funcionalidades avançadas de desenvolvimento e automação"
keywords: ["vibe", "mcp", "automation", "development", "tools", "integration"]
author: "Vibe"
---

# Onboarding

Antes de usar este power, valide que o usuário completou os seguintes passos.

## Step 1

Certifique-se de ter o Node.js instalado (versão 18 ou superior recomendada).

## Step 2

O Vibe MCP será instalado automaticamente via npx na primeira utilização.

# Overview

Vibe MCP fornece integração para funcionalidades avançadas de desenvolvimento e automação.

**Conexão**: Local via npx
**Package**: @vibe/mcp

## Available MCP Servers

### vibe
**Package:** @vibe/mcp
**Connection:** Local via npx

## Configuration

**MCP Configuration:**
```json
{
  "mcpServers": {
    "vibe": {
      "command": "npx",
      "args": ["@vibe/mcp"]
    }
  }
}
```

## Troubleshooting

**"Command not found"**: Verifique se o Node.js e npm estão instalados corretamente

**"Package not found"**: Verifique se o pacote @vibe/mcp está disponível no npm

**"Connection failed"**: Reinicie o servidor MCP e verifique os logs
