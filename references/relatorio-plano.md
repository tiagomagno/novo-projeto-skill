# Relatório do plano (obrigatório antes de gerar ou alterar qualquer arquivo)

Vale para **todos os modos** (projeto novo, existente e reestruturação). É a revisão final do usuário: ele vê **tudo** o que vai nascer ou mudar no projeto, e como o todo se encaixa, antes de qualquer arquivo ser gravado. Sem esse relatório aprovado, nada é gerado.

## Por que existe
Sem uma visão do todo, o projeto vira uma lista de tarefas soltas (uma tela aqui, outra ali) e ninguém consegue dizer como as partes se encaixam. As tarefas devem nascer das **regras de negócio e da arquitetura em camadas**, e não de telas isoladas.

## Quando apresentar
Depois da entrevista (`entrevista.md`) e da escolha de nível e de design system, e **antes** da geração. Apresente no chat, em partes legíveis (tabelas curtas, um diagrama simples). Termine com "Aprova este plano? O que quer mudar?" e **espere o ok**. Pedidos de ajuste: corrija o relatório e apresente de novo. Gere só depois do ok explícito.

## Antes de apresentar: verificação de consistência

Cruze os conteúdos que serão gerados e corrija as contradições **antes** de mostrar o relatório (o que não puder ser corrigido sozinho vira ❓ no item 10). Verifique, no mínimo:

- Toda tela (`SCR`) prevê uma ação que existe em alguma funcionalidade (`FEAT`)?
- Toda funcionalidade do MVP tem regra, tela (se houver interface) e tarefa?
- Todo dado que as funcionalidades exigem existe no modelo (`E`)? O modelo permite algum estado que as regras proíbem?
- A interface mostra alguma permissão que a matriz de papéis não prevê?
- A arquitetura depende de alguma integração (`INT`) que não foi especificada?
- O plano de tarefas deixa alguma funcionalidade do MVP sem cobertura?
- No perfil 3: toda entidade tem dono (organização) e o isolamento é aplicado no servidor, não só na tela?

Registre o resultado em uma linha ("7 cruzamentos, 2 correções, 1 ❓").

## Conteúdo (nesta ordem; remova só o que não se aplica)

0. **Perfil.** Porte escolhido (`perfis.md`), o que ele liga e desliga, e os módulos **avaliados e não aplicáveis** ("sem multi-tenant: um cliente só, confirmado"). Estratégia de Git proposta.
1. **Entendimento.** Problema, público, diferencial, alternativas, métrica de sucesso, modelo (pago, grátis, interno). Cada item com ✅/💡/❓.
2. **Visão do todo.** As **camadas** do sistema e o fluxo de dados entre elas (entrada → objeto/modelo central → motor/serviços → consumidores → saídas). Destaque o **objeto central** de que o resto deriva, se houver (ex.: o tema de uma marca, o pedido, o paciente). Inclua um diagrama (Mermaid ou texto) e a frase "o que depende do quê".
3. **Regras de negócio e modelo de domínio.** Tabela de regras (`RN`) e de entidades (`E`), com certeza e origem. Conforme o perfil: papéis e matriz de permissões, ciclo de vida das entidades com status (transições), integrações (`INT`) e regras de persistência. É a base das tarefas.
4. **Fluxos e telas.** Fluxos por tipo de usuário e a **lista de telas proposta**, ligadas aos fluxos. Ninguém lista telas do zero: você propõe, o usuário ajusta.
5. **Arquitetura e pastas.** Stack e estrutura de pastas. Em projeto existente: "como é hoje → como fica", com o motivo de cada mudança.
6. **O que será criado, alterado, movido ou preservado.** Tabela por arquivo/pasta:

   | Caminho | Ação (criar / alterar / mover / preservar) | O que muda | Por quê |
   |---|---|---|---|

   Inclua `CLAUDE.md`, `docs/`, `.claude/` (rules, commands, agents, hooks, settings), `.gitignore`, README, arquivos legados e qualquer movimentação. Nada fora desta tabela é tocado.
7. **Divisão de tarefas.** Fases e tarefas, **derivadas das regras de negócio e das camadas** (ver `tarefas-e-fases.md`), com:
   - a **ordem de construção** e as dependências ("a 4.2 depende da 3.0");
   - uma **matriz de cobertura**: cada RN aponta para as tarefas que a implementam, e cada tarefa cita pelo menos uma RN, fluxo ou camada. RN sem tarefa e tarefa sem RN são falhas do plano;
   - no perfil 3 (e nas regras críticas dos demais), uma coluna de **testes** (`TEST`): a regra só está coberta se há verificação prevista (automática ou manual) para ela. Veja a estratégia de testes em `tarefas-e-fases.md`.
8. **Specs previstas.** Quais tarefas terão spec (fluxo crítico, regra nova ou ambígua, várias camadas) e por quê. Por padrão a spec nasce quando a tarefa começa; se o usuário quiser, crie já na geração as das tarefas críticas.
9. **Agentes, regras e automações.** Quais agentes, rules, comandos e hooks serão criados e **para quê**, ligados ao que o projeto realmente tem (ex.: um agente que confere a consistência com um motor externo). Perguntados na entrevista (bloco 7), nunca presumidos.
10. **Perguntas ❓ em aberto e riscos.** O que ainda não se sabe, o que bloqueia qual tarefa, e o que pode dar errado.

## Depois do ok
- Gere os documentos, incluindo `docs/VISAO.md` (itens 2, 3 e 7 viram o mapa permanente do projeto; ver `templates-docs.md`).
- O restante do relatório (item 6, principalmente) fica registrado em `docs/DECISOES.md` como a decisão de estruturar o projeto, com a data, junto com: o perfil escolhido, a estratégia de Git e a hipótese 💡 "uma tarefa por sessão com `/clear` reduz o consumo do limite" (para comparar depois).
