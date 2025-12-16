---
name: "sequential-thinking"
displayName: "Sequential Thinking"
description: "Pensamento estruturado e sequencial para resolver problemas complexos - divida problemas em etapas, revise e ajuste seu raciocínio"
keywords: ["thinking", "reasoning", "problem-solving", "analysis", "planning", "sequential", "step-by-step"]
author: "Anthropic"
---

# Onboarding

Este power não requer configuração adicional. Ele executa localmente via npx.

## Step 1

O Sequential Thinking está pronto para uso imediato. Basta ativar o power e começar a usar para problemas complexos.

## Step 2 (Opcional)

Crie um hook para usar pensamento sequencial em decisões arquiteturais. Salve em .kiro/hooks/sequential-thinking.kiro.hook:

```json
{
  "enabled": true,
  "name": "Architecture Decision Helper",
  "description": "Usa pensamento sequencial para avaliar decisões arquiteturais",
  "version": "1",
  "when": {
    "type": "fileEdited",
    "patterns": [
      "docs/architecture/*.md",
      "docs/adr/*.md",
      "ARCHITECTURE.md"
    ]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Documento de arquitetura foi modificado. Use o Sequential Thinking para analisar a decisão, considerando trade-offs, alternativas e impactos."
  }
}
```

# Overview

Sequential Thinking permite raciocínio estruturado e iterativo para resolver problemas complexos. Divida problemas em etapas, revise seu pensamento e chegue a conclusões bem fundamentadas.

**Execução**: Local via npx

## Available MCP Servers

### sequential-thinking
**Package:** `@modelcontextprotocol/server-sequential-thinking`
**Connection:** stdio via npx

**Available Tools:**

**Thinking Management:**
- `sequentialthinking` - Ferramenta dinâmica para pensamento passo-a-passo

## Tool Usage Examples

```javascript
// Iniciar pensamento sobre um problema
mcp_sequential_thinking_sequentialthinking({
  "thought": "Preciso decidir entre usar REST ou GraphQL para a API",
  "thoughtNumber": 1,
  "totalThoughts": 5,
  "nextThoughtNeeded": true
})

// Continuar o raciocínio
mcp_sequential_thinking_sequentialthinking({
  "thought": "REST é mais simples e tem melhor caching, mas GraphQL evita over-fetching",
  "thoughtNumber": 2,
  "totalThoughts": 5,
  "nextThoughtNeeded": true
})

// Revisar um pensamento anterior
mcp_sequential_thinking_sequentialthinking({
  "thought": "Considerando que temos múltiplos clientes mobile, GraphQL pode ser melhor",
  "thoughtNumber": 3,
  "totalThoughts": 6,
  "nextThoughtNeeded": true,
  "isRevision": true,
  "revisesThought": 2
})

// Explorar alternativa
mcp_sequential_thinking_sequentialthinking({
  "thought": "Alternativa: usar REST com BFF (Backend for Frontend)",
  "thoughtNumber": 4,
  "totalThoughts": 6,
  "nextThoughtNeeded": true,
  "branchFromThought": 2,
  "branchId": "bff-alternative"
})

// Concluir
mcp_sequential_thinking_sequentialthinking({
  "thought": "Decisão: GraphQL com Apollo Server, pois atende melhor os múltiplos clientes",
  "thoughtNumber": 6,
  "totalThoughts": 6,
  "nextThoughtNeeded": false
})
```

## Workflows

**Análise de Problema:**
```javascript
// Passo 1: Definir o problema
await mcp_sequential_thinking_sequentialthinking({
  "thought": "Problema: A aplicação está lenta ao carregar a lista de produtos",
  "thoughtNumber": 1,
  "totalThoughts": 4,
  "nextThoughtNeeded": true
})

// Passo 2: Identificar causas possíveis
await mcp_sequential_thinking_sequentialthinking({
  "thought": "Causas possíveis: 1) Query N+1, 2) Falta de paginação, 3) Sem cache",
  "thoughtNumber": 2,
  "totalThoughts": 4,
  "nextThoughtNeeded": true
})

// Passo 3: Analisar evidências
await mcp_sequential_thinking_sequentialthinking({
  "thought": "Logs mostram 50+ queries por request - confirmado N+1",
  "thoughtNumber": 3,
  "totalThoughts": 4,
  "nextThoughtNeeded": true
})

// Passo 4: Propor solução
await mcp_sequential_thinking_sequentialthinking({
  "thought": "Solução: Implementar eager loading com include/join",
  "thoughtNumber": 4,
  "totalThoughts": 4,
  "nextThoughtNeeded": false
})
```

**Planejamento de Feature:**
```javascript
// Definir escopo
await mcp_sequential_thinking_sequentialthinking({
  "thought": "Feature: Sistema de notificações push",
  "thoughtNumber": 1,
  "totalThoughts": 5,
  "nextThoughtNeeded": true
})

// Listar componentes
await mcp_sequential_thinking_sequentialthinking({
  "thought": "Componentes: 1) Service Worker, 2) Backend API, 3) Queue de mensagens",
  "thoughtNumber": 2,
  "totalThoughts": 5,
  "nextThoughtNeeded": true
})

// Identificar dependências
await mcp_sequential_thinking_sequentialthinking({
  "thought": "Dependências: Firebase Cloud Messaging ou AWS SNS",
  "thoughtNumber": 3,
  "totalThoughts": 5,
  "nextThoughtNeeded": true
})

// Estimar esforço
await mcp_sequential_thinking_sequentialthinking({
  "thought": "Estimativa: 2 sprints - 1 para backend, 1 para frontend",
  "thoughtNumber": 4,
  "totalThoughts": 5,
  "nextThoughtNeeded": true
})

// Definir MVP
await mcp_sequential_thinking_sequentialthinking({
  "thought": "MVP: Notificações básicas sem personalização, apenas web",
  "thoughtNumber": 5,
  "totalThoughts": 5,
  "nextThoughtNeeded": false
})
```

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| thought | string | Yes | O pensamento atual |
| thoughtNumber | number | Yes | Número do pensamento na sequência |
| totalThoughts | number | Yes | Total estimado de pensamentos |
| nextThoughtNeeded | boolean | Yes | Se mais pensamentos são necessários |
| isRevision | boolean | No | Se este pensamento revisa um anterior |
| revisesThought | number | No | Número do pensamento sendo revisado |
| branchFromThought | number | No | Criar ramificação a partir de qual pensamento |
| branchId | string | No | Identificador da ramificação |

## Best Practices

- Comece com uma definição clara do problema
- Mantenha cada pensamento focado em um aspecto
- Use revisões quando descobrir novas informações
- Crie ramificações para explorar alternativas
- Ajuste totalThoughts conforme necessário
- Documente conclusões importantes

## Troubleshooting

**Pensamento muito longo**: Divida em múltiplos passos menores

**Perdeu o fio**: Use revisão para voltar e corrigir

**Muitas alternativas**: Use branches para organizar

## Configuration

**MCP Configuration:**
```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"],
      "env": {},
      "disabled": false
    }
  }
}
```
