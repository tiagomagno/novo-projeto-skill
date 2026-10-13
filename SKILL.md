---
name: novo-projeto
description: Cria, documenta ou reestrutura projetos (sites, sistemas, apps, APIs) com a estrutura base de contexto da equipe - CLAUDE.md, docs/ (PRODUTO, NEGOCIO, ARQUITETURA, DESIGN, VISAO, TASKS, ESTADO, DECISOES) e .claude/ (rules, commands, agents, hooks) - para que cada conversa nova saiba onde o projeto está. Conduz entrevista adaptativa a partir de uma ideia (texto ou áudio transcrito), lê projetos existentes e migra documentação fora do padrão. Use sempre que o usuário falar em novo projeto, iniciar projeto, estrutura base, contexto do projeto, PRD, documentar ou padronizar ou reestruturar um projeto existente, "tenho uma ideia de app/site/sistema", ou reclamar que o Claude não lembra do projeto entre conversas, mesmo sem citar a skill. Creates, documents or restructures projects with the team's base context structure via adaptive interview.
---

# Novo projeto: estrutura base com memória entre conversas

Esta skill monta (ou reconstrói) o contexto de um projeto para que qualquer conversa nova comece sabendo onde ele está. O contexto vive em arquivos Markdown versionados no repositório; a conversa é só a interface para preenchê-los.

## Por que esta estrutura existe

Conversas não carregam memória umas das outras, mas o repositório carrega. Por isso o desenho é:

- Um **retrato curto** (`docs/ESTADO.md`), importado pelo `CLAUDE.md`, que entra sozinho em toda conversa nova.
- **Documentos detalhados** (produto, negócio, arquitetura, design, tarefas, decisões) lidos sob demanda, só os relevantes para a tarefa.
- **Uma conversa por tarefa**, com início, meio e fim. O fechamento (atualizar tarefas, estado e decisões) é parte da tarefa e feito pelo Claude, não depende de o usuário lembrar de rodar um comando.
- **Marcação de certeza** em cada item (✅ confirmado, 💡 hipótese, ❓ em aberto), para que uma suposição nunca vire regra sem alguém perceber.

Quem chega com uma ideia não sabe responder "qual a stack?" ou "quais as regras de negócio?". A estrutura é o esquema de fundo; a entrevista, em linguagem de negócio, é a interface.

## Passo 0: detectar o modo

Olhe o diretório atual **antes** de perguntar qualquer coisa.

| Modo | Sinal | O que fazer |
|---|---|---|
| **A. Projeto novo** | Sem código nem docs, ou o usuário só descreve uma ideia | Entrevista completa |
| **B. Projeto existente** | Há código, sem documentação útil | Ler o repositório, montar rascunho em 💡, perguntar só o que o código não responde |
| **C. Reestruturação** | Há README/docs/CLAUDE.md/.claude fora do padrão | Mapear para a estrutura nova, preservar conteúdo, apontar lacunas |

Confirme em uma frase ("Parece um projeto existente com documentação parcial; vou tratar como reestruturação, certo?"). Se não houver acesso a um repositório (chat sem sistema de arquivos do projeto), use o modo A e entregue os arquivos gerados como arquivos para baixar.

## Fluxo

A entrevista e o relatório do plano são **obrigatórios em todos os modos** (novo, existente e reestruturação). Nenhum arquivo é gerado, alterado ou movido antes da aprovação do relatório.

1. **Descoberta.** Modo A: ouvir a ideia. Modos B e C: seguir `references/modos-existente.md` (inventário somente leitura).
2. **Entrevista completa.** Seguir `references/entrevista.md`, **todos os blocos**, em rodadas, até as perguntas prioritárias estarem respondidas (as regras de negócio decidem as tarefas). Extrair antes o que a pessoa já disse e o que o código/documentos já respondem, e levar isso **para confirmação**, nunca como substituto da pergunta. Não pular blocos por "o projeto já existe".
3. **Visão do todo.** Montar as camadas, o objeto central de que o resto deriva e o fluxo de dados entre elas (ver `references/relatorio-plano.md`, item 2). Confirmar com o usuário.
4. **Nível.** Enxuto ou completo (abaixo). Recomende com base nas respostas; só pergunte se ficar em dúvida.
5. **Design system**, se houver interface. Seguir `references/design-system.md` (quatro caminhos).
6. **Relatório do plano.** Apresentar tudo o que será criado, alterado, movido ou preservado, a visão do todo, as regras de negócio, a lista de telas, a divisão de tarefas com a matriz de cobertura, as specs previstas e os agentes propostos. Seguir `references/relatorio-plano.md`. **Esperar o ok explícito.**
7. **Geração.** Usar `references/templates-docs.md` (os oito documentos, incluindo `VISAO.md`) e, no nível completo, `references/templates-claude.md` (CLAUDE.md, rules, commands, agents, settings, hooks). Respeitar a geração condicional.
8. **Tarefas e fases.** Gerar o `TASKS.md` conforme `references/tarefas-e-fases.md`: tarefas **derivadas das regras de negócio e das camadas**, não de telas soltas. As specs previstas no relatório são criadas conforme combinado (por padrão sob demanda, ao iniciar a tarefa).
9. **Teste de contexto.** Ver abaixo.
10. **Relatório final.** Ver abaixo.

## Níveis

| | Enxuto | Completo |
|---|---|---|
| Quando | Site simples, protótipo, ideia em validação | Sistema ou app de cliente, com time |
| Gera | `CLAUDE.md` + oito documentos em `docs/` | Tudo do enxuto + `.claude/` (rules, commands, agents, hooks, settings) + `.mcp.json` se houver integrações |

Os oito documentos (`PRODUTO`, `NEGOCIO`, `ARQUITETURA`, `DESIGN`, `VISAO`, `TASKS`, `ESTADO`, `DECISOES`) sempre nascem, mesmo com itens ❓. Já a geração do restante é **condicional**:

- Sem API: não criar `rules/api-conventions.md`.
- Sem interface: não criar `rules/ui-mobile-first.md`, `agents/design-reviewer.md` nem `commands/revisar-ux.md`.
- Interface que não é mobile (ferramenta de desktop, painel interno): trocar `ui-mobile-first.md` por uma rule com as convenções visuais reais do projeto; registrar a troca em `DECISOES.md`.
- Agentes: criar só os que a entrevista (bloco 7) e o relatório do plano justificarem para este projeto; o trio code/design/security é apenas ponto de partida a confirmar.
- Sem deploy: não criar `skills/deploy/`.
- Projeto não público: não criar a seção de SEO em `PRODUTO.md`.
- Sem integrações externas: não criar `.mcp.json`.
- Specs de tarefa (`docs/tarefas/`) não são geradas na criação: nascem durante o desenvolvimento, só para tarefas grandes ou ambíguas.

## Teste de contexto

Ao terminar a geração, verifique se o contexto funciona:

1. Releia `CLAUDE.md` e confira que importa `@docs/ESTADO.md`, lista os comandos reais de build/teste e aponta para os demais documentos.
2. Escolha uma tarefa pequena do `TASKS.md` e descreva, em três linhas, quais arquivos de contexto ela precisaria ler e quais convenções aplicaria. Se a resposta sair genérica (sem stack, sem convenções, sem tokens de design), há lacuna: aponte qual arquivo está fraco.
3. Entregue ao usuário um teste para rodar numa **conversa nova**: "Onde paramos e qual a próxima tarefa?" A resposta deve citar fase, progresso e a próxima tarefa corretamente. Se não citar, o import do `ESTADO.md` não está funcionando.

## Relatório final

Termine sempre com:

- Arquivos criados e alterados (lista curta), conferidos contra a tabela aprovada no relatório do plano; qualquer diferença é explicada.
- Um lembrete de onde está a visão do todo (`docs/VISAO.md`) e a matriz de cobertura regras → tarefas.
- As até **5 perguntas ❓ mais importantes** ainda abertas.
- A **primeira tarefa sugerida** (normalmente a fundação 1.0 ou 2.0).
- Como usar no dia a dia: "Abra uma conversa por tarefa e diga 'vamos fazer a 2.3'. Ao concluir, o Claude atualiza tarefas, estado e decisões. Em tarefa grande ou ambígua, peça a spec primeiro: 'especifique a 3.1'."

## Regras de ouro

- **Nunca inventar fatos.** Informação sem fonte é 💡 (com a origem) ou ❓. O que se verifica no código (versões em manifestos, pastas, dependências) pode ser ✅.
- **Nunca sobrescrever sem confirmar.** Antes de alterar arquivo existente, mostre o que muda. Não faça commit automaticamente; sugira a mensagem.
- **Perguntas em linguagem de negócio**, sem jargão sem explicar. Em projeto novo, até três por vez; em reestruturação e projeto existente, rodadas de até quatro, quantas forem necessárias para fechar as regras de negócio. Quando a pessoa não souber, proponha uma hipótese e peça confirmação.
- **Nunca pular a entrevista nem o relatório do plano**, mesmo em projeto existente, mesmo com o código respondendo parte. Pergunte primeiro as decisões que mudam a divisão de tarefas; é melhor fechar regras de negócio do que gerar perguntas por tela depois.
- **Tarefas nascem das regras de negócio e das camadas**, nunca de uma lista de telas ou do histórico do Git. Telas são consequência.
- **Enxuto primeiro.** Texto genérico gasta contexto e não ajuda. Item sem informação vira uma linha ❓, nunca um parágrafo de enchimento.
- **Exemplos de código nas rules vêm do repositório real.** Se ainda não há código, deixe o espaço ❓ para preencher nas primeiras tarefas.
- **Segredos fora do contexto.** Nunca pedir nem gravar senhas ou chaves. `.env` entra no `deny` do `settings.json`.
- **Tamanho.** Documento acima de ~150 linhas: propor dividir em pasta (`docs/design/`, `docs/negocio/`).
- **Idioma.** PT-BR por padrão; acompanhar o idioma do usuário. Nomes de arquivo seguem o padrão desta skill.

## Arquivos de referência

Leia sob demanda, não todos de uma vez:

| Arquivo | Quando ler |
|---|---|
| `references/entrevista.md` | Sempre (modos A, B e C): a entrevista é obrigatória |
| `references/modos-existente.md` | Modos B e C (inventário e migração) |
| `references/relatorio-plano.md` | Antes de gerar qualquer arquivo, em todos os modos |
| `references/templates-docs.md` | Na geração dos oito documentos e ao criar a spec de uma tarefa |
| `references/templates-claude.md` | Na geração do `CLAUDE.md` e do `.claude/` (nível completo; o `CLAUDE.md` também no enxuto) |
| `references/tarefas-e-fases.md` | Ao gerar o `TASKS.md` e ao explicar o ciclo da tarefa |
| `references/design-system.md` | Quando o projeto tem interface |
