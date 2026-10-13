# Templates do `CLAUDE.md` e do `.claude/`

Preencha com dados reais do projeto. Nunca invente comandos, caminhos ou exemplos de código: leia do repositório ou deixe ❓.

## Sumário
1. CLAUDE.md
2. .gitignore
3. settings.json
4. rules/
5. commands/
6. agents/
7. hooks/
8. .mcp.json
9. Observações de ambiente (Windows)

---

## 1. CLAUDE.md (curto)

```markdown
# <NomeDoProjeto>
<Resumo em 2 linhas: o que é e para quem.>

@docs/ESTADO.md

## Comandos
- Instalar: `...` · Rodar: `...` · Testar: `...` · Lint: `...` · Build: `...`

## Documentação (ler antes de alterar algo relacionado)
- Visão do todo (camadas, o que depende do quê): docs/VISAO.md
- Produto: docs/PRODUTO.md · Negócio: docs/NEGOCIO.md
- Arquitetura: docs/ARQUITETURA.md · Design: docs/DESIGN.md
- Tarefas: docs/TASKS.md · Decisões: docs/DECISOES.md

## Fluxo de trabalho
- Uma conversa por tarefa. Comece lendo a tarefa em docs/TASKS.md e só os documentos relevantes; marque-a como 🔄.
- Ao concluir uma tarefa, **antes de declarar que terminou**: conferir o "pronto quando"; atualizar TASKS.md (✅, data, contadores) e o status dos itens cobertos; reescrever docs/ESTADO.md; registrar decisões novas em docs/DECISOES.md; sugerir commit com o ID da tarefa.
- Tarefa grande ou ambígua (fluxo crítico, regra de negócio nova, vários módulos): antes de implementar, criar a spec em docs/tarefas/<id>.md (objetivo sem tecnologia, critérios de aceite verificáveis, dúvidas ❓), resolver as dúvidas com o usuário e só então planejar e implementar.
- Trabalho novo que surgir no meio vira tarefa nova com ID novo; não infle a tarefa atual.
- Entregas que o usuário vê só ficam ✅ depois que ele testou.
- Não invente: o que não se sabe fica ❓ em ESTADO.md.
```

Mantenha o `CLAUDE.md` enxuto; o detalhe fica nos documentos. Em modo C, **mescle** com o `CLAUDE.md` existente em vez de substituir.

---

## 2. .gitignore (acrescentar)

```text
CLAUDE.local.md
.claude/settings.local.json
```

---

## 3. .claude/settings.json

```json
{
  "permissions": {
    "allow": [],
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Read(./secrets/**)"
    ]
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          { "type": "command", "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.sh" }
        ]
      }
    ],
    "SessionStart": [
      {
        "hooks": [
          { "type": "command", "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/session-start.sh" }
        ]
      }
    ]
  }
}
```

Ajustes por projeto: preencher `allow` com os comandos de build/teste/lint reais; acrescentar ao `deny` o que não deve ser lido (`node_modules`, `dist`, `.next` só se atrapalharem). Não ative hooks sem confirmar que o ambiente os executa (seção 9). Se o `settings.json` já existir, mescle sem remover o que há.

---

## 4. .claude/rules/

Cada arquivo de regras é curto e traz **exemplos reais do repositório** ("assim sim" e, se útil, "assim não"). Sem código ainda: deixe o espaço ❓.

### rules/geral.md (sempre existe)
```markdown
# Regras gerais
- Ler a documentação (PRODUTO, ARQUITETURA, DESIGN) antes de mudar algo relacionado.
- Mudanças pequenas e focadas; não mexer em arquivos não relacionados.
- Reaproveitar componentes, utilitários e serviços em vez de duplicar.
- Não criar pastas novas sem motivo claro e sem registrar em docs/ARQUITETURA.md.
- Código autoexplicativo, com nomes significativos.
- Validar entradas em cliente e servidor; nunca gravar segredos no código.
```

### rules/code-style.md
```markdown
# Estilo de código
<Linguagem, linter, formatador, convenções de nome de arquivo/componente/função.>

## Assim sim
<trecho real do repositório>
```

### rules/testing.md
```markdown
# Testes
<Ferramenta, onde ficam os testes (co-locados ou pasta), o que exige teste, meta de cobertura se houver.>

## Assim sim
<teste real do repositório>
```

### rules/api-conventions.md (só se houver API)
```markdown
# Convenções de API
<Padrão de rotas, formato de erro, validação, autenticação, versionamento.>
```

### rules/ui-mobile-first.md (só se houver interface; com escopo por caminho)
```markdown
---
paths:
  - "src/**/*.tsx"
---
# Interface mobile-first
- Projetar a partir de ~360 px; ampliar com `md:` e `lg:`.
- Usar apenas tokens e componentes do design system (ver docs/DESIGN.md); nada de cor ou espaçamento avulso.

## Checklist da tela
- [ ] Abre em 360 px sem rolagem lateral
- [ ] Áreas de toque de pelo menos 44 px
- [ ] Estados: vazio, carregando, erro e sucesso
- [ ] Só tokens e componentes do design system
- [ ] Textos no tom definido em docs/DESIGN.md
```
Ajuste `paths` para os arquivos de interface do projeto (a sintaxe exata de escopo por caminho deve ser conferida na versão do Claude Code em uso).

---

## 5. .claude/commands/ (atalhos opcionais)

O fluxo funciona só com a conversa e a regra do `CLAUDE.md`; os comandos são atalhos.

### commands/iniciar-tarefa.md
```markdown
---
description: Inicia uma tarefa lendo só o contexto necessário
argument-hint: [id da tarefa, ex.: 2.3]
---
Vamos fazer a tarefa $ARGUMENTS.
1. Leia a linha da tarefa em docs/TASKS.md (itens, "pronto quando") e a spec em docs/tarefas/$ARGUMENTS.md, se existir.
2. Leia apenas os documentos relevantes (tela: DESIGN; dados/regras: NEGOCIO e ARQUITETURA).
3. Marque a tarefa como 🔄 em docs/TASKS.md.
4. Resuma em até 5 linhas o que será feito e como será verificado; depois comece.
```

### commands/concluir-tarefa.md
```markdown
---
description: Fecha a tarefa atual atualizando tarefas, estado e decisões
argument-hint: [id da tarefa, opcional]
---
Conclua a tarefa $ARGUMENTS (ou a que está 🔄).
1. Confira o "pronto quando" (ou, se houver spec em docs/tarefas/, cada critério de aceite, um a um). Se algo não foi verificado, diga o que falta em vez de marcar ✅.
2. Atualize docs/TASKS.md (✅, data) e recalcule os contadores a partir das tabelas.
3. Atualize o status dos itens cobertos (ex.: telas no mapa de telas de docs/DESIGN.md).
4. Reescreva docs/ESTADO.md (curto).
5. Acrescente decisões novas em docs/DECISOES.md.
6. Sugira a mensagem de commit com o ID da tarefa.
```

### commands/especificar-tarefa.md
```markdown
---
description: Cria a spec leve de uma tarefa grande ou ambígua antes de implementar
argument-hint: [id da tarefa, ex.: 3.1]
---
Crie a spec da tarefa $ARGUMENTS em docs/tarefas/$ARGUMENTS.md. Não implemente nada.
1. Leia a linha da tarefa em docs/TASKS.md e os documentos relevantes (PRODUTO, NEGOCIO, DESIGN, ARQUITETURA).
2. Escreva primeiro o objetivo ("Como <ator>, quero <ação>, para <resultado>") e os critérios de aceite ("Dado..., quando..., então..."), **sem citar tecnologia**. Cada critério deve ser observável e testável; cite o ID da regra de negócio quando houver (RN3).
3. Liste o que ainda não se sabe como ❓ e pergunte ao usuário, até 3 perguntas por vez. Não planeje enquanto houver ❓ que mude o resultado.
4. Só depois, escreva o plano curto (módulos e arquivos afetados, abordagem, riscos) e as subtarefas (ex.: $ARGUMENTSa, $ARGUMENTSb).
5. Defina a verificação: o que é automático, o que o usuário confere manualmente.
6. Estrutura do arquivo: 1 Objetivo · 2 Critérios de aceite · 3 Fora do escopo · 4 Dúvidas · 5 Plano · 6 Subtarefas · 7 Verificação · 8 Resultado (vazio até concluir).
7. Em docs/TASKS.md, troque o "pronto quando" da tarefa por "ver spec". Peça a validação da spec antes de começar.
```

### commands/analisar.md
```markdown
---
description: Confere a consistência entre documentos, specs e tarefas (somente leitura)
argument-hint: [id da tarefa, opcional]
---
Faça uma análise **somente leitura**; não altere arquivos.
- Com id ($ARGUMENTS): confira a spec docs/tarefas/$ARGUMENTS.md contra PRODUTO, NEGOCIO, DESIGN e ARQUITETURA, antes de implementar.
- Sem id: confira o projeto inteiro.
Verifique:
- Funcionalidades do MVP (PRODUTO.md) cobertas por tarefas em TASKS.md, e tarefas sem funcionalidade correspondente.
- Regras de NEGOCIO.md refletidas nos critérios de aceite das specs.
- Telas do mapa de DESIGN.md com tarefa associada.
- Stack e pastas dos planos coerentes com ARQUITETURA.md.
- Contadores e status de TASKS.md coerentes com ESTADO.md.
- Itens ❓ que bloqueiam as próximas tarefas.
- Contradições entre documentos.
Entregue um relatório por gravidade (bloqueia / atenção / sugestão), com arquivo e trecho de cada achado e as perguntas que o usuário precisa responder.
```

### commands/revisar-ux.md (só se houver interface)
```markdown
---
description: Revisa uma tela contra o DESIGN.md e o checklist mobile-first
argument-hint: [id da tela ou caminho do arquivo]
---
Use o agente design-reviewer para revisar $ARGUMENTS contra docs/DESIGN.md e o checklist de .claude/rules/ui-mobile-first.md. Liste problemas por gravidade e sugira correções, sem aplicá-las.
```

### commands/review.md
```markdown
---
description: Revisão de código das alterações atuais
---
Use o agente code-reviewer para revisar as alterações atuais (git diff) contra .claude/rules/ e docs/ARQUITETURA.md.
```

### commands/fix-issue.md
```markdown
---
description: Corrige um problema descrito, de forma pequena e focada
argument-hint: [descrição ou número da issue]
---
Corrija: $ARGUMENTS. Reproduza primeiro, faça a menor mudança que resolve, rode os testes e registre como tarefa nova em docs/TASKS.md se o trabalho for maior que o previsto.
```

---

## 6. .claude/agents/ (somente leitura por padrão)

### agents/code-reviewer.md
```markdown
---
name: code-reviewer
description: Revisa código contra as regras do projeto. Use após mudanças relevantes ou ao concluir uma tarefa.
tools: Read, Grep, Glob, Bash
---
Você revisa código deste projeto. Leia .claude/rules/ e docs/ARQUITETURA.md, depois o diff atual (git diff). Aponte, por gravidade: erros, riscos, desvios das regras e duplicações. Cite arquivo e linha. Não altere arquivos.
```

### agents/design-reviewer.md (só se houver interface)
```markdown
---
name: design-reviewer
description: Revisa telas contra DESIGN.md e o design system. Use ao concluir tarefas de telas.
tools: Read, Grep, Glob
---
Você revisa interface. Leia docs/DESIGN.md e .claude/rules/ui-mobile-first.md. Confira: mobile-first (360 px), áreas de toque, estados vazio/carregando/erro/sucesso, uso exclusivo de tokens e componentes do design system, tom dos textos, acessibilidade básica. Liste problemas por gravidade com arquivo e linha. Não altere arquivos.
```

### agents/security-auditor.md
```markdown
---
name: security-auditor
description: Audita segurança básica (entradas, autenticação, segredos, dependências). Use em fluxos críticos e antes de entregas.
tools: Read, Grep, Glob
---
Você audita segurança. Procure: validação de entrada ausente, autenticação/autorização frouxas, segredos no código, exposição de dados pessoais, dependências suspeitas. Compare com os requisitos não funcionais de docs/PRODUTO.md. Liste achados por gravidade com arquivo e linha e a correção sugerida. Não altere arquivos.
```

---

## 7. .claude/hooks/ (scripts opcionais)

### hooks/validate-bash.sh
Bloqueia comandos perigosos antes de rodarem. Lista inicial; ajuste ao projeto.
```bash
#!/usr/bin/env bash
# PreToolUse (Bash): recebe o JSON do evento no stdin e bloqueia padrões perigosos.
input="$(cat)"
patterns=(
  "rm -rf /"
  "rm -rf ~"
  "rm -rf \*"
  "git push --force"
  "git push -f"
  "DROP DATABASE"
  "DROP TABLE"
  "mkfs"
  "chmod -R 777 /"
)
for p in "${patterns[@]}"; do
  if printf '%s' "$input" | grep -qiF -- "$p"; then
    echo "Bloqueado pelo hook validate-bash: comando contém '$p'. Confirme com o usuário antes de prosseguir." >&2
    exit 2
  fi
done
exit 0
```

### hooks/session-start.sh
Rede de segurança para conversas fechadas no meio: injeta, no início de cada conversa, as tarefas 🔄 e as alterações não commitadas.
```bash
#!/usr/bin/env bash
# SessionStart: o que for escrito no stdout entra como contexto da conversa.
cd "${CLAUDE_PROJECT_DIR:-.}" || exit 0
if [ -f docs/TASKS.md ]; then
  em_andamento="$(grep -F '🔄' docs/TASKS.md | head -n 10)"
  if [ -n "$em_andamento" ]; then
    echo "Tarefas em andamento (docs/TASKS.md):"
    echo "$em_andamento"
  fi
fi
if git rev-parse --is-inside-work-tree >/dev/null 2>&1; then
  alteracoes="$(git status --short | head -n 15)"
  if [ -n "$alteracoes" ]; then
    echo "Alterações não commitadas:"
    echo "$alteracoes"
  fi
fi
exit 0
```
Depois de criar os scripts, torne-os executáveis (`chmod +x .claude/hooks/*.sh`).

---

## 8. .mcp.json (somente se houver integrações)

Gere só os servidores que a entrevista confirmou (GitHub, Jira, Slack, Figma, banco). **Não grave tokens no arquivo**: use variáveis de ambiente e documente quais são necessárias em `docs/ARQUITETURA.md`. Confira a URL/comando oficial de cada servidor antes de gravar; se não tiver certeza, deixe a integração listada como ❓ em vez de adivinhar a configuração.

---

## 9. Observações de ambiente

- **Windows:** os scripts acima são Bash. Pergunte o sistema do time (entrevista, bloco 7). Em Windows, confirme que o shell que roda os hooks tem `bash` e `grep` disponíveis (por exemplo, Git Bash ou WSL) e teste cada hook uma vez antes de confiar nele. Se não rodarem, reescreva em PowerShell ou omita os hooks e avise o usuário.
- **Teste os hooks:** `validate-bash.sh` deve bloquear um comando de teste; `session-start.sh` deve imprimir as tarefas 🔄 numa conversa nova. Se algum comportamento diferir do esperado na versão do Claude Code em uso, ajuste ou desative e registre em `docs/DECISOES.md`.
- Ao instalar em projeto existente, **não sobrescreva** `.claude/` já presente; compare e proponha.
