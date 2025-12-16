---
name: "notion"
displayName: "Notion Workspace"
description: "Integração com Notion para gerenciamento de páginas, databases e conteúdo - crie wikis, documentação e bases de conhecimento programaticamente"
keywords: ["notion", "wiki", "documentation", "docs", "pages", "database", "knowledge-base", "notes", "workspace", "collaboration"]
author: "Notion"
---

# Onboarding

Antes de usar este power, valide que o usuário completou os seguintes passos.

## Step 1

O Notion MCP usa autenticação OAuth. Na primeira utilização, você será redirecionado para autorizar o acesso ao seu workspace Notion. Certifique-se de ter uma conta ativa em notion.so e selecione as páginas que deseja compartilhar com a integração.

## Step 2 (Opcional)

Crie um hook para sincronizar documentação com o código. Salve em .kiro/hooks/notion-docs.kiro.hook:

```json
{
  "enabled": true,
  "name": "Notion Documentation Sync",
  "description": "Atualiza documentação no Notion quando código é modificado",
  "version": "1",
  "when": {
    "type": "fileEdited",
    "patterns": [
      "README.md",
      "docs/**/*.md",
      "CHANGELOG.md",
      "API.md"
    ]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Documentação foi modificada. Verifique se há páginas correspondentes no Notion e atualize-as com as mudanças."
  }
}
```

# Overview

Notion MCP fornece integração completa com a plataforma Notion para gerenciamento de conteúdo, documentação e bases de conhecimento. Crie e gerencie páginas, databases, blocos e propriedades programaticamente.

**Conexão**: URL remota via mcp-remote
**Autenticação**: OAuth (autorização na primeira utilização)

## Available MCP Servers

### notion
**Package:** Notion MCP Service
**Connection:** URL remota via mcp-remote
**Endpoint:** https://mcp.notion.com/mcp

**Available Tools:**

**Page Management:**
- `notion_search` - Buscar páginas e databases
- `notion_get_page` - Obter detalhes de uma página
- `notion_create_page` - Criar nova página
- `notion_update_page` - Atualizar página existente
- `notion_archive_page` - Arquivar página

**Database Management:**
- `notion_get_database` - Obter detalhes de um database
- `notion_query_database` - Consultar items de um database
- `notion_create_database` - Criar novo database
- `notion_update_database` - Atualizar database

**Block Management:**
- `notion_get_block` - Obter bloco específico
- `notion_get_block_children` - Obter blocos filhos
- `notion_append_block_children` - Adicionar blocos a uma página
- `notion_update_block` - Atualizar bloco
- `notion_delete_block` - Remover bloco

**Comments:**
- `notion_get_comments` - Obter comentários
- `notion_create_comment` - Criar comentário

**Users:**
- `notion_get_user` - Obter usuário
- `notion_list_users` - Listar usuários

## Tool Usage Examples

```javascript
// Buscar páginas
mcp_notion_notion_search({
  "query": "documentação API"
})

// Criar página
mcp_notion_notion_create_page({
  "parent": { "page_id": "parent-page-id" },
  "properties": {
    "title": [{ "text": { "content": "Nova Documentação" } }]
  },
  "children": [
    {
      "object": "block",
      "type": "heading_1",
      "heading_1": {
        "rich_text": [{ "text": { "content": "Introdução" } }]
      }
    },
    {
      "object": "block",
      "type": "paragraph",
      "paragraph": {
        "rich_text": [{ "text": { "content": "Conteúdo da página..." } }]
      }
    }
  ]
})

// Consultar database
mcp_notion_notion_query_database({
  "database_id": "database-id",
  "filter": {
    "property": "Status",
    "select": { "equals": "Em Progresso" }
  }
})

// Adicionar blocos a página
mcp_notion_notion_append_block_children({
  "block_id": "page-id",
  "children": [
    {
      "object": "block",
      "type": "code",
      "code": {
        "rich_text": [{ "text": { "content": "console.log('Hello')" } }],
        "language": "javascript"
      }
    }
  ]
})
```

## Workflows

**Criar Documentação de Projeto:**
```javascript
// Criar página principal
const mainPage = await mcp_notion_notion_create_page({
  "parent": { "page_id": "workspace-root-id" },
  "properties": {
    "title": [{ "text": { "content": "Projeto X - Documentação" } }]
  },
  "children": [
    {
      "object": "block",
      "type": "heading_1",
      "heading_1": {
        "rich_text": [{ "text": { "content": "Visão Geral" } }]
      }
    },
    {
      "object": "block",
      "type": "paragraph",
      "paragraph": {
        "rich_text": [{ "text": { "content": "Descrição do projeto..." } }]
      }
    }
  ]
})

// Criar subpáginas
await mcp_notion_notion_create_page({
  "parent": { "page_id": mainPage.id },
  "properties": {
    "title": [{ "text": { "content": "Arquitetura" } }]
  }
})

await mcp_notion_notion_create_page({
  "parent": { "page_id": mainPage.id },
  "properties": {
    "title": [{ "text": { "content": "API Reference" } }]
  }
})
```

**Criar Database de Tasks:**
```javascript
// Criar database
const db = await mcp_notion_notion_create_database({
  "parent": { "page_id": "parent-page-id" },
  "title": [{ "text": { "content": "Tasks do Projeto" } }],
  "properties": {
    "Name": { "title": {} },
    "Status": {
      "select": {
        "options": [
          { "name": "To Do", "color": "gray" },
          { "name": "In Progress", "color": "blue" },
          { "name": "Done", "color": "green" }
        ]
      }
    },
    "Assignee": { "people": {} },
    "Due Date": { "date": {} },
    "Priority": {
      "select": {
        "options": [
          { "name": "High", "color": "red" },
          { "name": "Medium", "color": "yellow" },
          { "name": "Low", "color": "green" }
        ]
      }
    }
  }
})

// Adicionar item ao database
await mcp_notion_notion_create_page({
  "parent": { "database_id": db.id },
  "properties": {
    "Name": { "title": [{ "text": { "content": "Implementar login" } }] },
    "Status": { "select": { "name": "To Do" } },
    "Priority": { "select": { "name": "High" } }
  }
})
```

**Atualizar Documentação:**
```javascript
// Buscar página existente
const results = await mcp_notion_notion_search({
  "query": "API Reference"
})

const page = results.results[0]

// Adicionar novo conteúdo
await mcp_notion_notion_append_block_children({
  "block_id": page.id,
  "children": [
    {
      "object": "block",
      "type": "heading_2",
      "heading_2": {
        "rich_text": [{ "text": { "content": "Novo Endpoint" } }]
      }
    },
    {
      "object": "block",
      "type": "code",
      "code": {
        "rich_text": [{ "text": { "content": "GET /api/v1/users" } }],
        "language": "http"
      }
    }
  ]
})
```

## Block Types

Notion suporta vários tipos de blocos:

- **paragraph**: Texto simples
- **heading_1/2/3**: Títulos
- **bulleted_list_item**: Lista com bullets
- **numbered_list_item**: Lista numerada
- **to_do**: Checkbox
- **toggle**: Conteúdo colapsável
- **code**: Bloco de código
- **quote**: Citação
- **callout**: Destaque com ícone
- **divider**: Linha divisória
- **table**: Tabela
- **image/video/file**: Mídia

## Best Practices

- Organize conteúdo em hierarquia de páginas
- Use databases para dados estruturados
- Mantenha títulos descritivos
- Use templates para consistência
- Adicione ícones e covers para navegação visual
- Compartilhe páginas específicas, não o workspace inteiro

## Troubleshooting

**"Page not found"**: Verifique se a página foi compartilhada com a integração

**"Permission denied"**: Reautorize a integração com as páginas necessárias

**"Invalid block"**: Verifique a estrutura JSON do bloco

**"Database query failed"**: Verifique os nomes das propriedades e filtros

## Configuration

**MCP Configuration:**
```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.notion.com/mcp"]
    }
  }
}
```
