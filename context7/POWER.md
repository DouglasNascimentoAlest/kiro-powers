---
name: "context7"
displayName: "Context7 Documentation"
description: "Acesso a documentação atualizada de bibliotecas e frameworks - busque docs de qualquer tecnologia diretamente no contexto"
keywords: ["documentation", "docs", "libraries", "frameworks", "api", "reference", "context7", "search"]
author: "Context7"
---

# Onboarding

Este power não requer configuração adicional. Ele se conecta diretamente ao serviço Context7 via URL.

## Step 1

O Context7 está pronto para uso imediato. Basta ativar o power e começar a buscar documentação.

## Step 2 (Opcional)

Crie um hook para buscar documentação automaticamente quando trabalhar com novas dependências. Salve em .kiro/hooks/context7-docs.kiro.hook:

```json
{
  "enabled": true,
  "name": "Auto Documentation Lookup",
  "description": "Busca documentação automaticamente quando novas dependências são adicionadas",
  "version": "1",
  "when": {
    "type": "fileEdited",
    "patterns": [
      "package.json",
      "requirements.txt",
      "Pipfile",
      "pom.xml",
      "build.gradle",
      "go.mod",
      "Cargo.toml",
      "composer.json",
      "Gemfile"
    ]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Arquivo de dependências foi modificado. Use o Context7 para buscar documentação das novas bibliotecas adicionadas."
  }
}
```

# Overview

Context7 fornece acesso a documentação atualizada de bibliotecas e frameworks diretamente no contexto do agente. Busque referências de API, exemplos de código e guias de qualquer tecnologia.

**Conexão**: URL remota (https://mcp.context7.com/mcp)

## Available MCP Servers

### context7
**Package:** Context7 MCP Service
**Connection:** URL remota via SSE
**Endpoint:** https://mcp.context7.com/mcp

**Available Tools:**

**Documentation Search:**
- `resolve-library-id` - Resolver ID de uma biblioteca pelo nome
- `get-library-docs` - Obter documentação de uma biblioteca específica

## Tool Usage Examples

```javascript
// Resolver ID da biblioteca
mcp_context7_resolve_library_id({
  "libraryName": "react"
})

// Obter documentação
mcp_context7_get_library_docs({
  "context7CompatibleLibraryID": "/facebook/react",
  "topic": "hooks",
  "tokens": 5000
})

// Buscar docs de Express
mcp_context7_resolve_library_id({
  "libraryName": "express"
})

mcp_context7_get_library_docs({
  "context7CompatibleLibraryID": "/expressjs/express",
  "topic": "middleware"
})
```

## Workflows

**Aprender Nova Biblioteca:**
```javascript
// 1. Resolver o ID da biblioteca
const library = await mcp_context7_resolve_library_id({
  "libraryName": "nextjs"
})

// 2. Buscar documentação geral
const docs = await mcp_context7_get_library_docs({
  "context7CompatibleLibraryID": library.id,
  "tokens": 10000
})

// 3. Buscar tópico específico
const routingDocs = await mcp_context7_get_library_docs({
  "context7CompatibleLibraryID": library.id,
  "topic": "app router",
  "tokens": 5000
})
```

**Resolver Problema Específico:**
```javascript
// Buscar docs sobre autenticação no NextAuth
const authDocs = await mcp_context7_get_library_docs({
  "context7CompatibleLibraryID": "/nextauthjs/next-auth",
  "topic": "jwt session",
  "tokens": 3000
})
```

## Best Practices

- Use `resolve-library-id` primeiro para obter o ID correto
- Especifique `topic` para resultados mais relevantes
- Ajuste `tokens` baseado na complexidade do tópico
- Combine com Memory MCP para salvar informações importantes

## Troubleshooting

**"Library not found"**: Tente variações do nome (react vs reactjs)

**Documentação incompleta**: Aumente o valor de `tokens`

**Tópico não encontrado**: Use termos mais genéricos ou específicos

## Configuration

**MCP Configuration:**
```json
{
  "mcpServers": {
    "context7": {
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```
