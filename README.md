# Kiro Powers Collection

Coleção de Powers para o Kiro IDE - extensões que adicionam capacidades de MCP servers, documentação e workflows guiados.

## 🚀 Powers Disponíveis

| Power | Descrição |
|-------|-----------|
| **context7** | Acesso a documentação atualizada de bibliotecas e frameworks |
| **memory** | Memória persistente para contexto de longo prazo |
| **notion** | Integração com Notion para wikis e documentação |
| **monday** | Gerenciamento de projetos com Monday.com |
| **playwright** | Automação de browser e testes E2E |
| **sequential-thinking** | Pensamento estruturado para problemas complexos |

## 📦 Estrutura

```
kiro-powers/
├── context7/           # Documentação de libs via Context7
├── memory/             # Grafo de conhecimento persistente
├── notion/             # Integração Notion
├── monday/             # Integração Monday.com
├── playwright/         # Automação de browser
└── sequential-thinking/ # Raciocínio passo-a-passo
```

Cada power contém:
- `POWER.md` - Documentação completa e exemplos
- `mcp.json` - Configuração do MCP server
- `steering/` - Guias de workflow

## 🔧 Instalação

1. Clone este repositório na pasta de powers do Kiro
2. Os powers estarão disponíveis automaticamente no IDE
3. Ative cada power conforme necessário

## 📖 Uso

No Kiro, use o comando `kiroPowers` com:
- `action="list"` - Ver powers instalados
- `action="activate"` - Ativar e ver documentação
- `action="use"` - Executar ferramentas do power

## 🔑 Autenticação

| Power | Tipo |
|-------|------|
| context7 | Nenhuma (URL pública) |
| memory | Nenhuma (local) |
| notion | OAuth |
| monday | OAuth |
| playwright | Nenhuma (local) |
| sequential-thinking | Nenhuma (local) |
