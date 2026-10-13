# Modos B e C: projeto existente e reestruturação

O objetivo é documentar e padronizar sem perder nada do que já existe e sem inventar o que não se sabe. O repositório e os documentos antigos respondem boa parte das perguntas; a entrevista cobre o resto.

## Regras comuns

- **Porte primeiro.** A primeira pergunta continua sendo o porte (`perfis.md`): um projeto existente pequeno não ganha a documentação de uma plataforma. O perfil é confirmado com o usuário, não deduzido só do tamanho do código.
- **IDs existentes.** Se o projeto já usa outro esquema de IDs, mantenha-o e proponha a migração para o esquema de `perfis.md` no relatório do plano; só renumere com confirmação.
- **Somente leitura primeiro.** Faça o inventário inteiro antes de criar ou alterar qualquer arquivo.
- **Nada some.** Todo conteúdo útil de documentos antigos é preservado, com a origem anotada (`origem: README.md`).
- **Nunca sobrescrever sem confirmar.** Mostre o que muda e espere o ok. Originais só são movidos para `docs/_legado/` com confirmação; sem confirmação, ficam onde estão.
- **Fato verificável no código é ✅** (versão em manifesto, pasta, dependência). **Interpretação é 💡** (intenção de negócio, motivo de uma escolha, tarefa reconstruída). **Lacuna é ❓.**
- **Não commitar.** Sugira as mensagens de commit.

## Modo B: projeto existente sem documentação

### 1. Inventário
Leia, nesta ordem, o que existir:
- Árvore de pastas (ignorando `node_modules`, `dist`, `build`, `.next`, `vendor`, caches).
- Manifestos e configuração: `package.json`, `go.mod`, `requirements.txt`/`pyproject.toml`, `composer.json`, `pubspec.yaml`, `Dockerfile`, configuração de CI, `.env.example`.
- `README` e qualquer documentação solta.
- Esquemas de banco: migrações, `schema.prisma`, models, entidades.
- Rotas/endpoints e telas/páginas.
- Histórico do Git: `git log --oneline -n 80` e as datas do primeiro e do último commit.
- Testes existentes e como rodar (scripts de teste).

Nunca leia nem copie o conteúdo de `.env` ou de arquivos de segredos; use só o `.env.example`.

### 2. O que inferir
| Documento | Tirar do repositório |
|---|---|
| `ARQUITETURA.md` | Stack com versões (✅), diagrama a partir de pastas e dependências, mapa de pastas comentado |
| `NEGOCIO.md` | Entidades e relações a partir dos models/migrações (✅ estrutura, 💡 significado); fluxos prováveis a partir de rotas e telas (💡) |
| `DESIGN.md` | Telas existentes (mapa de telas, status ✅ se já implementadas), tokens/Tailwind/CSS existentes como origem do design system |
| `VISAO.md` | Camadas e fluxo de dados prováveis a partir de pastas, stores, módulos e dependências (💡), a confirmar na entrevista (bloco 1b) |
| `TASKS.md` | **A estrutura das fases vem das camadas e das regras de negócio** (confirmadas na entrevista), não do `git log`. O histórico do Git serve só para marcar o que já está feito (✅ com 💡 de origem); pendências do código (TODOs, telas incompletas) entram como tarefas ligadas a uma regra ou camada |
| `DECISOES.md` | Decisões prováveis a partir das escolhas visíveis ("usa Supabase para autenticação"), todas 💡 |
| `PRODUTO.md` | Quase nada do código: o que dá (nome, o que o app faz pelas telas) fica 💡; o resto é pergunta |
| `ESTADO.md` | Onde o projeto está, a partir do último trabalho no Git e do que está incompleto |

### 3. Entrevista completa
Depois do rascunho, rode a **entrevista inteira** (`entrevista.md`, todos os blocos): o rascunho vira "li isto no código, confere?", e as perguntas cobrem o que o repositório não diz (problema, público, diferencial, métricas, objeto central e camadas, regras de negócio, persistência, agentes, pastas, prioridade agora). Rodadas de até quatro perguntas, quantas forem necessárias.

### 4. Relatório do plano e geração
Monte o **relatório do plano** (`relatorio-plano.md`), com a tabela de tudo que será criado, alterado, movido ou preservado, e espere o ok. Só então gere os oito documentos com tudo marcado ✅/💡/❓.

## Modo C: reestruturação de projeto com documentação fora do padrão

### 1. Inventário de documentação
Liste tudo que for documentação: `README`, `docs/`, wiki exportada, `CLAUDE.md`, `.claude/`, arquivos como `CONTEXT.md`, `PRD`, `MEMORY`, notas soltas. Para cada um, anote tamanho e assunto.

### 2. Mapeamento
Monte uma tabela **antes** de mexer em qualquer coisa:

| Arquivo atual | Assunto | Vai para | Ação |
|---|---|---|---|
| README.md | Visão e como rodar | `PRODUTO.md` (visão), `CLAUDE.md` (comandos) | Copiar com origem |
| docs/arquitetura.md | Stack e pastas | `ARQUITETURA.md` | Migrar |
| notas.md | Decisões soltas | `DECISOES.md` | Migrar como entradas |

Mostre a tabela e peça confirmação.

### 3. Migrar
- Preserve o conteúdo no documento de destino, na seção certa, com a origem anotada.
- **Contradições** entre documentos, ou entre documento e código, **não se resolvem sozinhas**: registre as duas versões como ❓ e pergunte.
- **`CLAUDE.md` existente:** mescle. Mantenha as instruções do usuário, acrescente `@docs/ESTADO.md`, a lista de documentos e a regra de fechamento de tarefa. Não duplique o que já está lá.
- **`.claude/` existente:** não sobrescreva. Compare com os templates, mostre a diferença e proponha o que acrescentar (ex.: `geral.md`, comandos de tarefa, agentes que faltam).
- Documentos antigos que ficaram totalmente cobertos: proponha mover para `docs/_legado/` (com confirmação).

### 4. Relatório de lacunas
Entregue ao final:

| Documento | Situação | O que falta |
|---|---|---|
| PRODUTO.md | Parcial | Concorrentes, métricas, fora do escopo |
| NEGOCIO.md | Ausente | Regras e modelo de domínio |
| ... | ... | ... |

E uma lista curta de **conflitos encontrados** entre documentos e código. Em seguida, rode a **entrevista completa** (`entrevista.md`), começando pelas lacunas de maior impacto e pelos conflitos. Reestruturação é o momento de definir as regras de negócio: pode e deve ter muitas perguntas.

### 5. Relatório do plano
Monte o **relatório do plano** (`relatorio-plano.md`): o mapeamento antigo → novo, a visão do todo, as regras, as telas, a divisão de tarefas com a matriz de cobertura e tudo o que muda ("vai mudar disto para aquilo"). Só depois do ok explícito migre e gere.

## Fechamento (B e C)

Gere o `VISAO.md`, o `TASKS.md` e o `ESTADO.md` por último, já com o que foi aprovado no relatório. Rode o teste de contexto do `SKILL.md` e confira os arquivos criados contra a tabela aprovada. Registre em `DECISOES.md` a própria decisão de padronizar a estrutura, com a data.
