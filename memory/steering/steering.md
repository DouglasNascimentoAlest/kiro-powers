---
inclusion: always
---

# Armazenar Informações do Projeto
- Leia os arquivos principais do projeto (README, package.json, etc.)
- Crie entidades para o projeto, serviços e componentes principais
- Adicione observações relevantes sobre stack, configurações e decisões
- Crie relações entre as entidades

# Recuperar Contexto
- Use search_nodes para buscar informações relevantes
- Use open_nodes para detalhes de entidades específicas
- Use read_graph para visão geral completa

# Atualizar Memória
- Antes de criar novas entidades, verifique se já existem com search_nodes
- Use add_observations para adicionar novas informações a entidades existentes
- Use delete_observations para remover informações desatualizadas
- Mantenha o grafo organizado removendo entidades obsoletas

# Documentar Decisões
- Crie entidades do tipo "decision" para decisões arquiteturais
- Relacione decisões com os componentes afetados
- Adicione observações com justificativas e trade-offs
