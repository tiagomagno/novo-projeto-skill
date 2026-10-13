---
name: novo-projeto
description: Cria, documenta ou reestrutura projetos (sites, sistemas, apps, APIs, SaaS) com a estrutura base de contexto da equipe - CLAUDE.md, docs/ (PRODUTO, NEGOCIO, ARQUITETURA, DESIGN, VISAO, TASKS, ESTADO, DECISOES) e .claude/ (rules, commands, agents, hooks) - para que cada conversa nova saiba onde o projeto está. Conduz entrevista adaptativa ao porte do projeto a partir de uma ideia (texto ou áudio transcrito), cobrindo produto (PRD), requisitos técnicos (TRD), fluxos de telas, design system, banco de dados e plano de implementação com tarefas rastreáveis; lê projetos existentes e migra documentação fora do padrão. Use sempre que o usuário falar em novo projeto, iniciar projeto, estrutura base, contexto do projeto, PRD, TRD, plano de desenvolvimento, documentar ou padronizar ou reestruturar um projeto existente, "tenho uma ideia de app/site/sistema/SaaS", ou reclamar que o Claude não lembra do projeto entre conversas, mesmo sem citar a skill. Creates, documents or restructures projects with the team's base context structure via an interview adapted to project size.
---

# Novo projeto: estrutura base com memória entre conversas

Esta skill monta (ou reconstrói) o contexto de um projeto para que qualquer conversa nova comece sabendo onde ele está. O contexto vive em arquivos Markdown versionados no repositório; a conversa é só a interface para preenchê-los.

## Por que esta estrutura existe

Conversas não carregam memória umas das outras, mas o repositório carrega. Por isso o desenho é:

- Um **retrato curto** (`docs/ESTADO.md`), importado pelo `CLAUDE.md`, que entra sozinho em toda conversa nova.
- **Documentos detalhados** (produto, negócio, arquitetura, design, tarefas, decisões) lidos sob demanda, só os relevantes para a tarefa.
- **Uma tarefa por sessão**, com início, meio e fim. Tudo o que o Claude lê numa sessão fica nela e **vai inteiro para o modelo a cada mensagem**; encadear várias tarefas na mesma sessão faz o contexto crescer e gasta o limite de uso muito mais rápido. Por isso, ao concluir a tarefa, o Claude fecha o registro (tarefas, estado, decisões) e sugere `/clear` antes da próxima. Nada se perde, porque o estado vive nos arquivos.
- **Marcação de certeza** em cada item (✅ confirmado, 💡 hipótese, ❓ em aberto), para que uma suposição nunca vire regra sem alguém perceber.
- **IDs rastreáveis** (`FEAT`, `RN`, `E`, `SCR`, `API`, `INT`, tarefa, `TEST`) que ligam requisito, regra, tela, dado, tarefa e teste.

Quem chega com uma ideia não sabe responder "qual a stack?" ou "quais as regras de negócio?". A estrutura é o esquema de fundo; a entrevista, em linguagem de negócio, é a interface.

## Passo 0: detectar o modo e o porte

Olhe o diretório atual **antes** de perguntar qualquer coisa.

| Modo | Sinal | O que fazer |
|---|---|---|
| **A. Projeto novo** | Sem código nem docs, ou o usuário só descreve uma ideia | Entrevista completa |
| **B. Projeto existente** | Há código, sem documentação útil | Ler o repositório, montar rascunho em 💡, perguntar só o que o código não responde |
| **C. Reestruturação** | Há README/docs/CLAUDE.md/.claude fora do padrão | Mapear para a estrutura nova, preservar conteúdo, apontar lacunas |

Confirme em uma frase ("Parece um projeto existente com documentação parcial; vou tratar como reestruturação, certo?"). Se não houver acesso a um repositório (chat sem sistema de arquivos do projeto), use o modo A e entregue os arquivos gerados como arquivos para baixar.

Depois do modo, **a primeira pergunta da entrevista é o porte do projeto** (`references/perfis.md`): simples, aplicação ou plataforma (SaaS/multi-tenant). O perfil decide a profundidade de tudo o que vem a seguir. Não presuma que todo projeto precisa do fluxo completo.

## Fluxo

A entrevista e o relatório do plano são **obrigatórios em todos os modos** (novo, existente e reestruturação), na profundidade do perfil. Nenhum arquivo é gerado, alterado ou movido antes da aprovação do relatório.

1. **Descoberta.** Modo A: ouvir a ideia. Modos B e C: seguir `references/modos-existente.md` (inventário somente leitura).
2. **Porte e perfil.** Perguntar o porte e fixar o perfil (`references/perfis.md`).
3. **Entrevista.** Seguir `references/entrevista.md` (blocos 0 a 7) e, conforme o perfil, os aprofundamentos A a G de `references/entrevista-aprofundada.md`, em rodadas, até as perguntas prioritárias estarem respondidas (as regras de negócio decidem as tarefas). Extrair antes o que a pessoa já disse e o que o código/documentos já respondem, e levar isso **para confirmação**, nunca como substituto da pergunta. Não pular blocos do perfil por "o projeto já existe".
4. **Visão do todo.** Montar as camadas, o objeto central de que o resto deriva e o fluxo de dados entre elas (ver `references/relatorio-plano.md`, item 2). Confirmar com o usuário.
5. **Design system**, se houver interface. Seguir `references/design-system.md` (quatro caminhos; se não houver nada, as perguntas guia).
6. **Verificação de consistência.** Antes de apresentar o plano, cruzar os documentos que seriam gerados (lista em `references/relatorio-plano.md`) e corrigir as contradições.
7. **Relatório do plano.** Apresentar tudo o que será criado, alterado, movido ou preservado, o perfil, a visão do todo, as regras de negócio, a lista de telas, a divisão de tarefas com a matriz de cobertura, as specs previstas e os agentes propostos. Seguir `references/relatorio-plano.md`. **Esperar o ok explícito.**
8. **Geração.** Usar `references/templates-docs.md` (os oito documentos, incluindo `VISAO.md`), `references/templates-avancados.md` (seções que o perfil pede) e, no nível completo, `references/templates-claude.md` (CLAUDE.md, rules, commands, agents, settings, hooks). Respeitar a geração condicional.
9. **Tarefas e fases.** Gerar o `TASKS.md` conforme `references/tarefas-e-fases.md`: tarefas **derivadas das regras de negócio e das camadas**, não de telas soltas. As specs previstas no relatório são criadas conforme combinado (por padrão sob demanda, ao iniciar a tarefa).
10. **Teste de contexto.** Ver abaixo.
11. **Relatório final.** Ver abaixo.

## Perfis e níveis

O **perfil** (porte) define a profundidade; o **nível** define quais arquivos nascem.

| Perfil | Nível de arquivos | Resumo |
|---|---|---|
| 1. Simples (site, protótipo, projeto pessoal) | Enxuto: `CLAUDE.md` + oito documentos em `docs/` | Entrevista curta; só os IDs `RN`, `FEAT`, `SCR`; Git só `main` |
| 2. Aplicação (app com login e dados) | Completo: tudo do enxuto + `.claude/` (rules, commands, agents, hooks, settings) + `.mcp.json` se houver integrações | Catálogo de funcionalidades, papéis, estados, dados e integrações |
| 3. Plataforma (SaaS, várias empresas) | Completo | + multi-tenant, planos, segurança e ambientes completos, matriz regra → tarefa → teste |

Detalhes e a tabela completa de módulos por perfil estão em `references/perfis.md`.

Os oito documentos (`PRODUTO`, `NEGOCIO`, `ARQUITETURA`, `DESIGN`, `VISAO`, `TASKS`, `ESTADO`, `DECISOES`) sempre nascem, mesmo com itens ❓. Já a geração do restante é **condicional**:

- Sem API: não criar `rules/api-conventions.md`.
- Sem interface: não criar `rules/ui-mobile-first.md`, `agents/design-reviewer.md` nem `commands/revisar-ux.md`.
- Interface que não é mobile (ferramenta de desktop, painel interno): trocar `ui-mobile-first.md` por uma rule com as convenções visuais reais do projeto; registrar a troca em `DECISOES.md`.
- Sem banco: não criar as seções "Modelo de dados" e "Regras de persistência" em `NEGOCIO.md` nem "Migrações" em `ARQUITETURA.md`.
- Um só cliente e sem organizações: não criar a seção multi-tenant de `NEGOCIO.md` (depois de **perguntar**, não por omissão).
- Agentes: criar só os que a entrevista (bloco 7) e o relatório do plano justificarem para este projeto; o trio code/design/security é apenas ponto de partida a confirmar.
- Sem deploy: não criar `skills/deploy/`.
- Projeto não público: não criar a seção de SEO em `PRODUTO.md`.
- Sem integrações externas: não criar `.mcp.json` nem o inventário de integrações.
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
- Como usar no dia a dia: "Abra **uma sessão por tarefa** e diga 'vamos fazer a 2.3'. Ao concluir, o Claude atualiza tarefas, estado e decisões, e você roda `/clear` antes da próxima (nada se perde: o estado está nos arquivos, e a sessão curta gasta menos do seu limite). Em tarefa grande ou ambígua, peça a spec primeiro: 'especifique a 3.1'. Se um requisito mudar, diga 'mudou X': o Claude lista o que isso afeta antes de alterar."

## Regras de ouro

- **Nunca inventar fatos.** Informação sem fonte é 💡 (com a origem) ou ❓. O que se verifica no código (versões em manifestos, pastas, dependências) pode ser ✅.
- **Nunca sobrescrever sem confirmar.** Antes de alterar arquivo existente, mostre o que muda. **Não crie branches, commits nem PRs sem pedido do usuário**; sugira a mensagem de commit.
- **Perguntar ou propor conforme o risco** (`references/perfis.md`): decisão que afeta arquitetura, segurança, dados pessoais, dinheiro ou regra de negócio é perguntada ou fica ❓; decisão de baixo risco é proposta (💡), explicada em uma linha.
- **Perguntas em linguagem de negócio**, sem jargão sem explicar. Em projeto novo, até três por vez; em reestruturação e projeto existente, rodadas de até quatro, quantas forem necessárias para fechar as regras de negócio. Quando a pessoa não souber, proponha uma hipótese e peça confirmação.
- **Nunca pular a entrevista nem o relatório do plano**, mesmo em projeto existente, mesmo com o código respondendo parte. Pergunte primeiro as decisões que mudam a divisão de tarefas; é melhor fechar regras de negócio do que gerar perguntas por tela depois.
- **Tarefas nascem das regras de negócio e das camadas**, nunca de uma lista de telas ou do histórico do Git. Telas são consequência.
- **Documentos sincronizados.** Descobrir algo num documento que muda outro (um dado que falta no modelo, uma regra que a tela contradiz) exige revisar os dois, não só o que se estava editando.
- **Enxuto primeiro.** Texto genérico gasta contexto e não ajuda. Item sem informação vira uma linha ❓, nunca um parágrafo de enchimento. Módulo avaliado e não aplicável é registrado como "não se aplica".
- **Exemplos de código nas rules vêm do repositório real.** Se ainda não há código, deixe o espaço ❓ para preencher nas primeiras tarefas.
- **Segredos fora do contexto.** Nunca pedir nem gravar senhas ou chaves. `.env` entra no `deny` do `settings.json`.
- **Tamanho.** Documento acima de ~150 linhas: propor dividir em pasta (`docs/design/`, `docs/negocio/`).
- **Idioma.** PT-BR por padrão; acompanhar o idioma do usuário. Nomes de arquivo seguem o padrão desta skill.

## Arquivos de referência

Leia sob demanda, não todos de uma vez:

| Arquivo | Quando ler |
|---|---|
| `references/perfis.md` | Sempre, logo no início: porte, perfil, Git, IDs |
| `references/entrevista.md` | Sempre (modos A, B e C): a entrevista é obrigatória |
| `references/entrevista-aprofundada.md` | Perfis 2 e 3, só os blocos A a G que o perfil pede |
| `references/modos-existente.md` | Modos B e C (inventário e migração) |
| `references/relatorio-plano.md` | Antes de gerar qualquer arquivo, em todos os modos |
| `references/templates-docs.md` | Na geração dos oito documentos e ao criar a spec de uma tarefa |
| `references/templates-avancados.md` | Na geração das seções que o perfil pede (módulos, permissões, estados, dados, integrações, segurança, navegação, tokens) |
| `references/templates-claude.md` | Na geração do `CLAUDE.md` e do `.claude/` (nível completo; o `CLAUDE.md` também no enxuto) |
| `references/tarefas-e-fases.md` | Ao gerar o `TASKS.md` e ao explicar o ciclo da tarefa |
| `references/design-system.md` | Quando o projeto tem interface |
