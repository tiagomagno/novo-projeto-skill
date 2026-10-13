# Tarefas, fases e ciclo de uma conversa por tarefa

## Ciclo de uma tarefa

Cada tarefa do `TASKS.md` é feita em **uma conversa**, com início, meio e fim.

**Início.** O usuário diz "vamos fazer a 2.3". O `CLAUDE.md` já importou `docs/ESTADO.md`, então o retrato geral está carregado. Leia a linha da tarefa em `TASKS.md` e só os documentos relevantes (tela: `DESIGN.md`; dados: `NEGOCIO.md` e `ARQUITETURA.md`; regra de negócio: `NEGOCIO.md`). Marque a tarefa como 🔄. Se a tarefa tem spec (`docs/tarefas/<id>.md`), leia-a primeiro: os critérios de aceite dela substituem o "pronto quando" de uma linha.

**Meio.** Trabalhe seguindo `.claude/rules/` e use os agentes de revisão quando aplicável. Trabalho novo que surgir vira **ID novo**; a tarefa atual não infla.

**Fim (feito pelo Claude, antes de declarar a tarefa pronta).**
1. Conferir o "pronto quando" (ou, se há spec, cada critério de aceite, um a um).
2. Atualizar `TASKS.md` (✅ e data) e recalcular os contadores a partir das tabelas.
3. Reescrever `ESTADO.md`.
4. Acrescentar decisões novas em `DECISOES.md`.
5. Atualizar o status dos itens cobertos (por exemplo, telas no mapa de telas do `DESIGN.md`).
6. Sugerir commit com o ID: `feat(2.3): rotas protegidas`.

**Rede de segurança** (não depende de o usuário lembrar de nada):
- A regra do `CLAUDE.md` exige o fechamento antes de concluir.
- A tarefa já nasce 🔄: se a conversa for fechada no meio, o estado mostra o que ficou pela metade, e a conversa seguinte retoma com `git status` e `git diff`.
- O commit com ID é um segundo registro, independente dos arquivos.
- No nível completo, o hook `session-start.sh` injeta tarefas 🔄 e alterações não commitadas no início de cada conversa (ver `templates-claude.md`).

## Fases e agrupamento

**Fase** é um grupo de tarefas com relação direta entre si. Vale para qualquer parte construída: telas, módulos de backend, banco, integrações, deploy.

Regras de agrupamento:
- Agrupar por **fluxo, módulo ou feature**: itens que compartilham componentes, dados ou lógica são feitos juntos.
- Tamanho típico: **2 a 4 itens por tarefa**, o que cabe em uma conversa. Se não couber, divida.
- Cada fase com base compartilhada começa com uma tarefa **x.0 de fundação**. Sem ela, cada conversa inventa a própria base e o resultado sai inconsistente.
- Trabalho que surgir no meio, e correções depois do teste, viram **ID novo**; a tarefa original não é reaberta.
- Cada tarefa tem um **"pronto quando"** verificável e não depende de decisões ainda ❓. Se depender, a decisão vira tarefa anterior.

### Padrão por tipo de entrega

| Tipo | Fundação (x.0) | Agrupa por | Verificação no "pronto quando" |
|---|---|---|---|
| **Modelo central / motor** | Modelo do objeto central e contrato com o motor (com fallback) | Camada ou regra de derivação | Valores batem com o motor de referência; ajustes manuais preservados |
| **Telas** | Tokens e componentes base do design system, **ligados ao modelo central** | Fluxo de usuário (T1, T2...) | 360 px sem rolagem lateral, áreas de toque de 44 px, estados vazio/carregando/erro, revisão do `design-reviewer`, **teste manual do usuário** no celular |
| **API / backend** | Padrão de rotas, erros, validação e autenticação | Módulo ou domínio | Testes de unidade e de contrato, erros tratados, metas dos requisitos não funcionais |
| **Banco / dados** | Esquema base, migrações e dados de exemplo | Entidade ou domínio | Migração aplica e reverte sem perda |
| **Integrações** | Cliente, credenciais e configuração | Serviço externo | Chamada de teste em ambiente seguro e falhas tratadas |
| **Deploy / operação** | Pipeline e ambientes | Ambiente | Deploy em homologação e rollback testado |

Entregas que o usuário vê (telas, fluxos) só ficam ✅ depois que **ele** testou rodando. Fluxos críticos (cadastro, pagamento) ganham testes automatizados de ponta a ponta como **tarefa separada**.

### Rastreabilidade

A coluna "Itens" da tarefa referencia os IDs do que ela cobre (telas T1-T3, entidades E1, endpoints). Ao concluir, atualize o status desses itens.

## Spec leve por tarefa (sob demanda)

A spec evita que o agente adivinhe requisitos não escritos em tarefas que importam. As tarefas que terão spec são **listadas no relatório do plano** (e justificadas). Por padrão a spec nasce **ao iniciar a tarefa**; se o usuário pedir no relatório, as das tarefas críticas são criadas já na geração. Só quando faz diferença.

**Quando criar:**
- Fluxo crítico (cadastro, pagamento, dados sensíveis).
- Regra de negócio nova ou ambígua.
- Tarefa que não cabe em um "pronto quando" de uma linha.
- Tarefa que toca mais de um módulo.
- O usuário pediu.

Tarefa pequena e clara continua só com o "pronto quando".

**Fluxo da tarefa com spec:**
1. **Especificar:** objetivo e critérios de aceite, **sem citar tecnologia** (o quê e por quê, antes do como).
2. **Esclarecer:** tudo o que ainda não se sabe vira ❓; perguntar ao usuário (até 3 por vez). Não planejar enquanto houver ❓ que mude o resultado.
3. **Planejar:** plano curto (módulos e arquivos afetados, abordagem, riscos, dependências).
4. **Subtarefas:** quebrar em passos pequenos (ex.: 3.1a, 3.1b).
5. **Implementar** seguindo `rules/`.
6. **Verificar** cada critério de aceite, um a um; entregas visíveis passam pelo teste manual do usuário.
7. **Fechar:** preencher o resultado da spec, atualizar `TASKS.md`, `ESTADO.md` e `DECISOES.md` como no ciclo normal.

**Regra de fonte única:** o `TASKS.md` continua sendo a única fonte das tarefas. Com spec, o "pronto quando" da tarefa diz "ver spec" e os critérios vivem só na spec, sem duplicar. Subtarefas da spec não entram no painel de contadores.

**Consistência (`/analisar`):** antes de implementar uma tarefa com spec, e de vez em quando no projeto todo, confira a coerência entre `PRODUTO`, `NEGOCIO`, `DESIGN`, `ARQUITETURA`, specs e `TASKS`. É uma análise somente leitura que entrega um relatório por gravidade.

## Como gerar o `TASKS.md`

**Princípio:** as tarefas nascem das **regras de negócio (RN) e das camadas** do sistema (`VISAO.md`), nunca de uma lista de telas nem do histórico do Git. Uma tela entra como consequência: ela consome uma camada e aplica RN. Se a divisão parece "uma tarefa por tela", falta a fundação (o modelo ou o motor de que as telas dependem) e o plano está errado.

1. Parta das **camadas** e do **objeto central** (`VISAO.md`) e das RN e fluxos (`NEGOCIO.md`).
2. **Fase 1: Setup.** Repositório, ambiente, lint/formatação, `.env.example`. (Em projeto existente, já nasce ✅ ou 💡 conforme o que o repositório mostra.)
3. **Fundações do modelo central e dos serviços** como fases próprias, **antes** dos consumidores (telas, relatórios): modelo, motor/integração, persistência, autenticação, cada uma com x.0.
4. **Uma fase por camada ou módulo consumidor** (telas por fluxo, módulos de backend por domínio), cada uma com x.0 e tarefas de 2 a 4 itens, **dependendo explicitamente** das fundações.
5. **Fase final: Qualidade e entrega.** Testes de ponta a ponta dos fluxos críticos, acessibilidade, performance (metas dos requisitos não funcionais), deploy se houver.
6. Numerar fase.tarefa (2.3), preencher prioridade, "pronto quando", itens e **RN cobertas**. Calcular o painel de contadores a partir das tabelas.
7. **Matriz de cobertura:** cada RN aponta para as tarefas que a implementam e cada tarefa cita ao menos uma RN, fluxo ou camada. RN sem tarefa e tarefa sem RN são falhas do plano; corrija antes de apresentar o relatório.
8. No modo B/C, o histórico do Git serve **só para marcar o que já está feito** (✅ com 💡 na coluna de origem até o usuário confirmar); ele não define a estrutura das fases.
