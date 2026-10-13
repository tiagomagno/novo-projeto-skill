# Perfil de profundidade e IDs rastreáveis

A skill não faz o mesmo trabalho em todo projeto. A **primeira pergunta da entrevista** define o perfil, e o perfil liga ou desliga módulos. Um site de uma página não precisa de matriz de permissões; um SaaS com várias empresas precisa.

## Pergunta de porte (sempre a primeira)

Em linguagem de negócio, sem jargão:

> "Para eu ajustar o tamanho da documentação: isso é (1) um site ou página simples, um protótipo ou teste; (2) um app ou sistema com login e dados guardados; ou (3) um sistema usado por várias empresas ou clientes, com planos, permissões ou cobrança?"

Se a pessoa hesitar, proponha o perfil mais provável (💡) e peça confirmação. O perfil pode **subir** no meio da entrevista (ex.: "ah, cada cliente terá a própria conta" muda de 2 para 3); registre a mudança em `DECISOES.md`.

## Os três perfis

| | **1. Simples** | **2. Aplicação** | **3. Plataforma (SaaS / multi-tenant)** |
|---|---|---|---|
| Exemplos | Site institucional, landing page, protótipo, script, projeto pessoal | App com login, painel interno, sistema de um cliente, API | SaaS, sistema para várias empresas, produto com planos |
| Nível dos arquivos | Enxuto (`CLAUDE.md` + oito documentos) | Completo | Completo |
| Entrevista | Blocos 0, 1, 2, 4 (telas e design system), 5 curto | Blocos 0 a 7 | Blocos 0 a 7 + aprofundamentos A a G |
| Funcionalidades | Lista simples com prioridade | Catálogo `FEAT` agrupado por módulo, com critérios de aceite | Idem + matriz de permissões e planos |
| Papéis e permissões | Só se houver área restrita | Papéis e o que cada um faz | Matriz completa, hierarquia, escopo por organização |
| Ciclo de vida (estados) | Só se houver | Para as entidades com status | Para todas, com quem pode cada transição |
| Dados | Só se houver banco | Entidades, relações, dicionário resumido, regras de exclusão | Dicionário completo, migrações, isolamento entre organizações, backup |
| Integrações | Lista | Tabela detalhada | Tabela detalhada + falha, limites, custo, idempotência |
| Segurança | Linha nos requisitos não funcionais | Seção de segurança proporcional ao risco | Seção completa (auditoria, segredos, backup, LGPD) |
| Ambientes e deploy | Onde publica | Local + produção, e homologação se houver time | Local, homologação, produção, rollback |
| Estratégia de testes | "Pronto quando" da tarefa | + testes por regra crítica | + matriz regra → tarefa → teste |
| IDs rastreáveis | `RN`, `FEAT`, `SCR` | Todos os da lista abaixo | Todos |
| Git | Só `main` | `main` + `dev`; `feature/*` quando houver time | Fluxo completo (ver abaixo) |

Os módulos do perfil 3 que não se aplicam a um projeto específico ficam marcados "não se aplica" no relatório do plano, **depois de avaliados**, nunca por omissão.

## Quando perguntar e quando propor

- **Perguntar** (ou registrar como ❓ e bloquear a tarefa) quando a informação afeta **arquitetura, segurança, dados pessoais, dinheiro ou regra de negócio**: ex.: "um usuário pode pertencer a mais de uma empresa?", "o que acontece com os dados ao cancelar a assinatura?".
- **Propor e registrar como 💡** quando a decisão é de **baixo risco e fácil de reverter**: ex.: biblioteca de ícones, nome de pasta, formato de data. Explique a escolha em uma linha e siga.
- Nunca tomar em silêncio uma decisão do primeiro grupo. Nunca fazer dez perguntas sobre o segundo.

## Estratégia de Git por porte

Pergunte (bloco 7) e registre em `DECISOES.md`. O padrão sugerido, **a confirmar com o usuário**:

| Porte | Branches |
|---|---|
| Simples | Só `main`. |
| Aplicação | `main` + `dev`. `feature/*` quando houver mais de uma pessoa ou tarefas em paralelo. |
| Plataforma | `feature/* → dev → pre-prod → main`, com PR para `dev`, `pre-prod` e `main`; `fix/*`, `hotfix/*`, `refactor/*` conforme a convenção da equipe. |

Em qualquer porte: nunca push direto em `main` de projeto com equipe ou produção, e a skill **não cria branches, commits nem PRs sem pedido do usuário**. Convenção de commit sugerida: `tipo(escopo): descrição`, citando o ID da tarefa quando houver (`feat(2.3): rotas protegidas`). Se o usuário ou o projeto já tem convenção própria, ela vence.

## IDs rastreáveis

Todo item importante ganha um ID estável, para responder "o que depende disto?" e "quem verifica esta regra?". Dois dígitos; passe para três se passar de 99.

| Prefixo | O que identifica | Onde nasce |
|---|---|---|
| `FEAT-01` | Funcionalidade | `PRODUTO.md` |
| `RN-01` | Regra de negócio | `NEGOCIO.md` |
| `E-01` | Entidade do modelo de dados | `NEGOCIO.md` |
| `SCR-01` | Tela | `DESIGN.md` |
| `API-01` | Endpoint ou contrato de serviço | `ARQUITETURA.md` |
| `INT-01` | Integração externa | `ARQUITETURA.md` |
| `2.3` | Tarefa (fase.tarefa) | `TASKS.md` |
| `TEST-01` | Teste ou verificação manual registrada | spec da tarefa ou `TASKS.md` |

Regras:
- IDs **nunca são reaproveitados**: item removido fica como "descartado", não cede o número.
- No perfil Simples, use só `RN`, `FEAT` e `SCR`.
- Em projeto existente (modos B e C) que já tem outro esquema (`F1`, `T1`), **mantenha o esquema existente** e proponha a migração para o novo no relatório do plano; só renumere com confirmação.
- A cadeia completa, quando o perfil pede: `FEAT-01 → RN-01 → SCR-03 → API-02 → E-01 → 2.3 → TEST-04`.
