---
name: "playwright"
displayName: "Browser Automation with Playwright"
description: "Automação de browser e testes E2E com Playwright - navegue, interaja e capture screenshots de páginas web programaticamente"
keywords: ["playwright", "browser", "automation", "testing", "e2e", "web", "scraping", "screenshots", "selenium"]
author: "Microsoft"
---

# Onboarding

Este power não requer configuração adicional. Ele executa localmente via npx.

## Step 1

O Playwright MCP está pronto para uso imediato. Na primeira execução, pode baixar os browsers necessários automaticamente.

## Step 2 (Opcional)

Crie um hook para executar testes E2E automaticamente. Salve em .kiro/hooks/playwright-test.kiro.hook:

```json
{
  "enabled": true,
  "name": "E2E Test Runner",
  "description": "Executa testes E2E quando componentes de UI são modificados",
  "version": "1",
  "when": {
    "type": "fileEdited",
    "patterns": [
      "src/pages/**/*.tsx",
      "src/components/**/*.tsx",
      "src/views/**/*.vue",
      "app/**/*.tsx"
    ]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Componente de UI foi modificado. Use o Playwright para verificar se a página ainda funciona corretamente - navegue até ela e verifique os elementos principais."
  }
}
```

# Overview

Playwright MCP fornece automação completa de browser para testes E2E, web scraping e interação com páginas web. Suporta Chromium, Firefox e WebKit.

**Execução**: Local via npx
**Browsers**: Chromium (padrão), Firefox, WebKit

## Available MCP Servers

### playwright
**Package:** `@playwright/mcp`
**Connection:** stdio via npx

**Available Tools:**

**Navigation:**
- `browser_navigate` - Navegar para uma URL
- `browser_go_back` - Voltar na história
- `browser_go_forward` - Avançar na história
- `browser_wait` - Aguardar tempo ou condição

**Interaction:**
- `browser_click` - Clicar em elemento
- `browser_type` - Digitar texto em campo
- `browser_fill` - Preencher campo de input
- `browser_select` - Selecionar opção em dropdown
- `browser_hover` - Passar mouse sobre elemento
- `browser_press_key` - Pressionar tecla
- `browser_drag` - Arrastar elemento

**Content:**
- `browser_snapshot` - Capturar snapshot acessível da página
- `browser_screenshot` - Capturar screenshot
- `browser_pdf` - Gerar PDF da página
- `browser_get_text` - Obter texto de elemento

**Tabs:**
- `browser_tab_new` - Abrir nova aba
- `browser_tab_close` - Fechar aba
- `browser_tab_list` - Listar abas abertas
- `browser_tab_select` - Selecionar aba

**Files:**
- `browser_file_upload` - Upload de arquivo

**Utilities:**
- `browser_console` - Executar JavaScript no console
- `browser_network` - Monitorar requisições de rede

## Tool Usage Examples

```javascript
// Navegar para página
mcp_playwright_browser_navigate({
  "url": "https://example.com"
})

// Preencher formulário de login
mcp_playwright_browser_fill({
  "selector": "#email",
  "value": "user@example.com"
})

mcp_playwright_browser_fill({
  "selector": "#password",
  "value": "senha123"
})

mcp_playwright_browser_click({
  "selector": "button[type='submit']"
})

// Aguardar navegação
mcp_playwright_browser_wait({
  "selector": ".dashboard"
})

// Capturar screenshot
mcp_playwright_browser_screenshot({
  "path": "screenshot.png",
  "fullPage": true
})

// Obter texto de elemento
mcp_playwright_browser_get_text({
  "selector": ".welcome-message"
})
```

## Workflows

**Teste de Login:**
```javascript
// Navegar para página de login
await mcp_playwright_browser_navigate({
  "url": "http://localhost:3000/login"
})

// Preencher credenciais
await mcp_playwright_browser_fill({
  "selector": "[name='email']",
  "value": "test@example.com"
})

await mcp_playwright_browser_fill({
  "selector": "[name='password']",
  "value": "testpassword"
})

// Submeter formulário
await mcp_playwright_browser_click({
  "selector": "button[type='submit']"
})

// Verificar redirecionamento
await mcp_playwright_browser_wait({
  "selector": ".dashboard",
  "timeout": 5000
})

// Capturar evidência
await mcp_playwright_browser_screenshot({
  "path": "login-success.png"
})
```

**Web Scraping:**
```javascript
// Navegar para página
await mcp_playwright_browser_navigate({
  "url": "https://news.ycombinator.com"
})

// Capturar snapshot acessível
const snapshot = await mcp_playwright_browser_snapshot({})

// Obter texto de elementos
const titles = await mcp_playwright_browser_get_text({
  "selector": ".titleline"
})
```

**Teste de Formulário:**
```javascript
// Navegar
await mcp_playwright_browser_navigate({
  "url": "http://localhost:3000/contact"
})

// Preencher campos
await mcp_playwright_browser_fill({
  "selector": "#name",
  "value": "João Silva"
})

await mcp_playwright_browser_fill({
  "selector": "#email",
  "value": "joao@example.com"
})

await mcp_playwright_browser_fill({
  "selector": "#message",
  "value": "Mensagem de teste"
})

// Selecionar opção
await mcp_playwright_browser_select({
  "selector": "#subject",
  "value": "support"
})

// Submeter
await mcp_playwright_browser_click({
  "selector": "button[type='submit']"
})

// Verificar sucesso
await mcp_playwright_browser_wait({
  "selector": ".success-message"
})
```

## Selectors

O Playwright suporta vários tipos de seletores:

- **CSS**: `button`, `.class`, `#id`, `[attribute='value']`
- **Text**: `text=Click me`, `text="Exact match"`
- **Role**: `role=button[name='Submit']`
- **XPath**: `xpath=//button[@type='submit']`
- **Combinados**: `article >> text=Read more`

## Best Practices

- Use seletores estáveis (data-testid, role) em vez de classes CSS
- Aguarde elementos antes de interagir
- Capture screenshots para debugging
- Use timeouts apropriados
- Feche abas não utilizadas

## Troubleshooting

**"Element not found"**: Verifique o seletor, aguarde o elemento carregar

**"Timeout"**: Aumente o timeout ou verifique se a página carregou

**"Browser not installed"**: Execute `npx playwright install`

**"Navigation failed"**: Verifique a URL e conectividade

## Configuration

**MCP Configuration:**
```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

**Com browser específico:**
```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--browser", "firefox"]
    }
  }
}
```
