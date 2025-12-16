---
name: "monday"
displayName: "Monday.com Project Management"
description: "Integração com Monday.com para gerenciamento de projetos - crie boards, items, atualize status e gerencie workflows programaticamente"
keywords: ["monday", "project-management", "tasks", "boards", "workflow", "kanban", "sprint", "pipeline", "crm", "collaboration", "work-os"]
author: "Monday.com"
---

# Onboarding

Antes de usar este power, valide que o usuário completou os seguintes passos.

## Step 1

O Monday.com MCP usa autenticação OAuth. Na primeira utilização, você será redirecionado para autorizar o acesso à sua conta Monday.com. Certifique-se de ter uma conta ativa em monday.com.

## Step 2 (Opcional)

Crie um hook para sincronizar tarefas com o código. Salve em .kiro/hooks/monday-sync.kiro.hook:

```json
{
  "enabled": true,
  "name": "Monday Task Sync",
  "description": "Sincroniza progresso de tarefas com Monday.com quando código é commitado",
  "version": "1",
  "when": {
    "type": "fileEdited",
    "patterns": [
      "src/**/*.ts",
      "src/**/*.tsx",
      "src/**/*.js",
      "src/**/*.jsx"
    ]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Código foi modificado. Verifique se há items no Monday.com relacionados a esta mudança e atualize o status se necessário."
  }
}
```

# Overview

Monday.com MCP fornece integração completa com a plataforma Monday.com para gerenciamento de projetos, tarefas e workflows. Crie e gerencie boards, items, colunas e automações programaticamente.

**Conexão**: URL remota via mcp-remote
**Autenticação**: OAuth (autorização na primeira utilização)

## Available MCP Servers

### monday
**Package:** Monday.com MCP Service
**Connection:** URL remota via mcp-remote
**Endpoint:** https://mcp.monday.com/mcp

**Available Tools:**

**Board Management:**
- `list_boards` - Listar todos os boards
- `get_board` - Obter detalhes de um board
- `create_board` - Criar novo board
- `update_board` - Atualizar board existente
- `delete_board` - Remover board

**Item Management:**
- `list_items` - Listar items de um board
- `get_item` - Obter detalhes de um item
- `create_item` - Criar novo item
- `update_item` - Atualizar item existente
- `delete_item` - Remover item
- `move_item` - Mover item entre grupos

**Column Management:**
- `list_columns` - Listar colunas de um board
- `create_column` - Criar nova coluna
- `update_column_value` - Atualizar valor de coluna

**Group Management:**
- `list_groups` - Listar grupos de um board
- `create_group` - Criar novo grupo
- `update_group` - Atualizar grupo

**User & Workspace:**
- `get_me` - Obter usuário atual
- `list_users` - Listar usuários
- `list_workspaces` - Listar workspaces

## Tool Usage Examples

```javascript
// Listar boards
mcp_monday_list_boards({})

// Obter detalhes de um board
mcp_monday_get_board({
  "board_id": "1234567890"
})

// Criar item
mcp_monday_create_item({
  "board_id": "1234567890",
  "group_id": "new_group",
  "item_name": "Implementar feature de login"
})

// Atualizar status
mcp_monday_update_column_value({
  "board_id": "1234567890",
  "item_id": "9876543210",
  "column_id": "status",
  "value": "{\"label\": \"Working on it\"}"
})

// Mover item para outro grupo
mcp_monday_move_item({
  "item_id": "9876543210",
  "group_id": "done_group"
})
```

## Workflows

**Setup de Projeto:**
```javascript
// Criar board para o projeto
const board = await mcp_monday_create_board({
  "board_name": "Projeto X - Sprint 1",
  "board_kind": "public"
})

// Criar grupos
await mcp_monday_create_group({
  "board_id": board.id,
  "group_name": "Backlog"
})

await mcp_monday_create_group({
  "board_id": board.id,
  "group_name": "In Progress"
})

await mcp_monday_create_group({
  "board_id": board.id,
  "group_name": "Done"
})

// Criar items iniciais
await mcp_monday_create_item({
  "board_id": board.id,
  "group_id": "backlog",
  "item_name": "Setup do projeto"
})
```

**Tracking de Feature:**
```javascript
// Criar item para feature
const item = await mcp_monday_create_item({
  "board_id": "1234567890",
  "group_id": "backlog",
  "item_name": "Sistema de autenticação"
})

// Atualizar com detalhes
await mcp_monday_update_column_value({
  "board_id": "1234567890",
  "item_id": item.id,
  "column_id": "text",
  "value": "Implementar login com email/senha e OAuth"
})

// Atribuir responsável
await mcp_monday_update_column_value({
  "board_id": "1234567890",
  "item_id": item.id,
  "column_id": "person",
  "value": "{\"personsAndTeams\": [{\"id\": 12345, \"kind\": \"person\"}]}"
})

// Definir deadline
await mcp_monday_update_column_value({
  "board_id": "1234567890",
  "item_id": item.id,
  "column_id": "date",
  "value": "{\"date\": \"2024-12-31\"}"
})
```

**Relatório de Status:**
```javascript
// Obter todos os items
const items = await mcp_monday_list_items({
  "board_id": "1234567890"
})

// Filtrar por status
const inProgress = items.filter(i => i.status === "Working on it")
const done = items.filter(i => i.status === "Done")

// Gerar relatório
console.log(`Em progresso: ${inProgress.length}`)
console.log(`Concluídos: ${done.length}`)
```

## Column Types

Monday.com suporta vários tipos de colunas:

- **status**: Status com labels coloridos
- **person**: Atribuição de pessoas
- **date**: Datas e deadlines
- **text**: Texto livre
- **numbers**: Valores numéricos
- **timeline**: Período de tempo
- **link**: URLs
- **checkbox**: Marcação simples

## Best Practices

- Organize items em grupos lógicos (Backlog, In Progress, Done)
- Use status consistentes em todos os boards
- Atribua responsáveis a todos os items
- Defina deadlines realistas
- Mantenha descrições atualizadas
- Use automações para tarefas repetitivas

## Troubleshooting

**"Board not found"**: Verifique o ID e permissões de acesso

**"Column not found"**: Liste colunas do board para obter IDs corretos

**"Permission denied"**: Verifique se tem acesso ao workspace

**"Invalid value"**: Verifique o formato JSON do valor da coluna

## Configuration

**MCP Configuration:**
```json
{
  "mcpServers": {
    "monday": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.monday.com/mcp"]
    }
  }
}
```
