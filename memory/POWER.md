---
name: "memory"
displayName: "Persistent Memory"
description: "Gerenciamento de memória persistente para contexto de longo prazo - armazene e recupere informações entre sessões de forma estruturada"
keywords: ["memory", "persistence", "context", "knowledge", "storage", "recall", "entities", "relations"]
author: "Anthropic"
---

# Onboarding

Antes de usar este power, valide que o usuário completou os seguintes passos.

## Step 1

Configure o caminho do arquivo de memória. O usuário pode definir a variável de ambiente `MEMORY_FILE_PATH` no sistema ou configurar diretamente no arquivo de configuração MCP (geralmente em ~/.kiro/settings/mcp.json). Se não configurado, o servidor usará um arquivo padrão no diretório atual.

## Step 2

Crie um hook para salvar automaticamente informações importantes. Salve o hook em .kiro/hooks/memory-save.kiro.hook:

```json
{
  "enabled": true,
  "name": "Memory Auto-Save",
  "description": "Salva automaticamente informações importantes do projeto na memória persistente",
  "version": "1",
  "when": {
    "type": "fileEdited",
    "patterns": [
      "README.md",
      "package.json",
      "requirements.txt",
      ".env.example",
      "docker-compose.yml"
    ]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Arquivo de configuração do projeto foi modificado. Use o Memory MCP para atualizar as entidades e relações do projeto na memória persistente."
  }
}
```

# Overview

O Memory MCP Server fornece gerenciamento de memória persistente baseado em um grafo de conhecimento. Permite armazenar entidades, suas observações e relações entre elas, mantendo contexto entre sessões.

**Armazenamento**: Arquivo JSON local configurável via MEMORY_FILE_PATH

## Available MCP Servers

### memory
**Package:** `@modelcontextprotocol/server-memory`
**Connection:** stdio via npx
**Armazenamento:** Arquivo JSON local

**Available Tools:**

**Entity Management:**
- `create_entities` - Criar novas entidades no grafo de conhecimento
- `delete_entities` - Remover entidades do grafo
- `add_observations` - Adicionar observações a entidades existentes
- `delete_observations` - Remover observações específicas de entidades

**Relation Management:**
- `create_relations` - Criar relações entre entidades
- `delete_relations` - Remover relações entre entidades

**Search & Retrieval:**
- `search_nodes` - Buscar nós no grafo de conhecimento
- `open_nodes` - Abrir nós específicos por nome
- `read_graph` - Ler o grafo de conhecimento completo

## Tool Usage Examples

```javascript
// Criar entidades
mcp_memory_create_entities({
  "entities": [
    {
      "name": "ProjectX",
      "entityType": "project",
      "observations": ["Framework React", "Backend Node.js", "Database PostgreSQL"]
    },
    {
      "name": "UserService",
      "entityType": "service",
      "observations": ["Gerencia autenticação", "Usa JWT tokens"]
    }
  ]
})

// Criar relações
mcp_memory_create_relations({
  "relations": [
    {
      "from": "ProjectX",
      "to": "UserService",
      "relationType": "contains"
    }
  ]
})

// Buscar no grafo
mcp_memory_search_nodes({
  "query": "autenticação"
})

// Adicionar observações
mcp_memory_add_observations({
  "observations": [
    {
      "entityName": "UserService",
      "contents": ["Implementado rate limiting", "Suporta OAuth2"]
    }
  ]
})

// Ler grafo completo
mcp_memory_read_graph({})
```

## Workflows

**Documentar Projeto:**
```javascript
// Criar entidade do projeto
await mcp_memory_create_entities({
  "entities": [{
    "name": "MeuProjeto",
    "entityType": "project",
    "observations": ["Stack: React + Node", "Deploy: AWS", "CI/CD: GitHub Actions"]
  }]
})

// Adicionar serviços
await mcp_memory_create_entities({
  "entities": [
    { "name": "API", "entityType": "service", "observations": ["REST API", "Port 3000"] },
    { "name": "Frontend", "entityType": "service", "observations": ["React SPA", "Port 5173"] }
  ]
})

// Relacionar
await mcp_memory_create_relations({
  "relations": [
    { "from": "MeuProjeto", "to": "API", "relationType": "has_service" },
    { "from": "MeuProjeto", "to": "Frontend", "relationType": "has_service" },
    { "from": "Frontend", "to": "API", "relationType": "depends_on" }
  ]
})
```

**Recuperar Contexto:**
```javascript
// Buscar informações relevantes
const results = await mcp_memory_search_nodes({ "query": "autenticação" })

// Abrir nós específicos
const nodes = await mcp_memory_open_nodes({ "names": ["UserService", "API"] })

// Ver grafo completo
const graph = await mcp_memory_read_graph({})
```

## Best Practices

- Use nomes descritivos e únicos para entidades
- Organize entidades por tipo (project, service, component, person, concept)
- Mantenha observações concisas e específicas
- Crie relações significativas entre entidades
- Atualize observações quando o projeto evoluir
- Use search_nodes para encontrar informações antes de criar duplicatas

## Troubleshooting

**"Entity not found"**: Verifique o nome exato da entidade com read_graph

**Arquivo não persiste**: Verifique se MEMORY_FILE_PATH está configurado corretamente

**Duplicatas**: Use search_nodes antes de criar novas entidades

## Configuration

**MCP Configuration:**
```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"],
      "env": {
        "MEMORY_FILE_PATH": "/path/to/memory.json"
      }
    }
  }
}
```
