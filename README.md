# novo-projeto

Skill do Claude que cria, documenta ou reestrutura projetos (sites, sistemas, apps, APIs) com uma estrutura base de contexto, para que cada conversa nova saiba onde o projeto está.

Gera `CLAUDE.md`, `docs/` (PRODUTO, NEGOCIO, ARQUITETURA, DESIGN, VISAO, TASKS, ESTADO, DECISOES) e `.claude/` (rules, commands, agents, hooks), a partir de uma entrevista adaptativa. Funciona em três modos: projeto novo, projeto existente e reestruturação.

## Estrutura

- `SKILL.md`: fluxo principal da skill.
- `references/`: entrevista, modos para projeto existente, relatório do plano, templates, tarefas e fases, design system.

## Instalação

Copie esta pasta para o diretório de skills do Claude (por exemplo `~/.claude/skills/novo-projeto`).
