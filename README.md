# novo-projeto

Skill do Claude que cria, documenta ou reestrutura projetos (sites, sistemas, apps, APIs, SaaS) com uma estrutura base de contexto, para que cada conversa nova saiba onde o projeto está.

Gera `CLAUDE.md`, `docs/` (PRODUTO, NEGOCIO, ARQUITETURA, DESIGN, VISAO, TASKS, ESTADO, DECISOES) e `.claude/` (rules, commands, agents, hooks), a partir de uma entrevista adaptativa. Funciona em três modos: projeto novo, projeto existente e reestruturação.

## Como funciona

- **Perfil de profundidade.** A primeira pergunta é o porte do projeto (simples, aplicação ou plataforma/SaaS). O perfil liga ou desliga módulos: papéis e permissões, multi-tenant, ciclo de vida, dados, integrações, segurança, ambientes e testes.
- **Entrevista em linguagem de negócio**, cobrindo produto (PRD), requisitos técnicos (TRD), fluxos de telas, design system, banco de dados e plano de implementação.
- **Relatório do plano** aprovado antes de gerar qualquer arquivo, com checagem de consistência entre documentos.
- **IDs rastreáveis** (`FEAT`, `RN`, `E`, `SCR`, `API`, `INT`, tarefa, `TEST`) e matriz regra → tarefa → teste.
- **Uma tarefa por sessão**: ao concluir, o Claude atualiza tarefas, estado e decisões e sugere `/clear`, porque o contexto da sessão é reenviado a cada mensagem.
- **Mudança de requisito** passa por análise de impacto antes de editar.

## Estrutura

- `SKILL.md`: fluxo principal.
- `references/perfis.md`: porte, perfis, Git por porte e esquema de IDs.
- `references/entrevista.md` e `entrevista-aprofundada.md`: entrevista base e blocos A a G.
- `references/modos-existente.md`: projeto existente e reestruturação.
- `references/relatorio-plano.md`: relatório do plano e verificação de consistência.
- `references/templates-docs.md` e `templates-avancados.md`: modelos dos documentos e seções por perfil.
- `references/templates-claude.md`: `CLAUDE.md`, rules, commands, agents, hooks.
- `references/tarefas-e-fases.md`: tarefas, fases, testes e ciclo de uma sessão.
- `references/design-system.md`: caminhos do design system e perguntas guia.

## Instalação

Copie esta pasta para o diretório de skills do Claude (por exemplo `~/.claude/skills/novo-projeto`).
