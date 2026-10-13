# Entrevista adaptativa

A estrutura é o esquema de fundo; a conversa é a interface. A pessoa pode chegar só com uma ideia, dita em áudio ou digitada de qualquer jeito. Seu trabalho é transformar isso em conteúdo para os documentos, perguntando apenas o que falta.

## Como conduzir

1. **Extrair antes de perguntar.** Leia tudo o que a pessoa disse (inclusive texto transcrito de áudio, com erros de transcrição) e preencha mentalmente os campos que já dá. Não repergunte nada que ela já respondeu.
2. **Perguntar em rodadas.** Em projeto novo, de uma a três perguntas por vez; em projeto existente ou reestruturação, rodadas de até quatro, quantas forem necessárias. Sempre em linguagem de negócio: "Quem vai usar isso no dia a dia?" em vez de "defina as personas". **A entrevista cobre todos os blocos em todos os modos**; o que o código ou os documentos já respondem entra como "entendi assim, confere?", nunca como pergunta pulada.
3. **Propor quando a pessoa não souber.** Ofereça uma hipótese concreta ("Pelo que você descreveu, parece que o principal usuário é o dono do mercado. Faz sentido?") e registre como 💡 até confirmar.
4. **Seguir a ordem abaixo**, mas pular blocos já respondidos e voltar a blocos quando a conversa trouxer informação nova.
5. **Registrar a certeza** de cada informação: ✅ a pessoa afirmou, 💡 você inferiu ou ela hesitou, ❓ ninguém sabe ainda.
6. **Prioridade primeiro, sem encurtar o essencial.** Ordene as perguntas pelo que mais muda a divisão de tarefas: (a) o **objeto central** de que o resto deriva e as camadas, (b) as **regras de negócio** e o modelo de domínio, (c) usuários, permissões e persistência, (d) saídas e integrações, (e) time, agentes e pastas. Essas **não podem ser puladas**: ou são respondidas, ou o usuário decide explicitamente deixá-las ❓ (e o relatório do plano mostra o que isso bloqueia). Perguntas de acabamento (telas, textos, detalhes visuais) podem ficar para a spec da tarefa. Se a pessoa estiver cansada, pare só depois das prioritárias; um projeto com ❓ honestos vale mais que um com respostas inventadas.
7. **Regras de negócio decidem as tarefas.** Prefira fechar uma regra ("como o tema nasce da logo? o que o usuário pode sobrescrever?") a gerar, depois, uma pergunta por tela. Cada regra confirmada vira RN e será coberta por tarefa.

## Blocos e perguntas

### 1. Negócio (alimenta `PRODUTO.md`)
- Qual problema isso resolve, e para quem?
- Como essas pessoas resolvem isso hoje? (Alternativas, concorrentes, planilha, papel.)
- Por que trocariam pela solução nova? O que seria o diferencial?
- Como saber que deu certo? (Uma ou duas métricas simples.)
- O cliente final quer pagar, usar de graça, ou é uso interno?

Esta é a **pergunta de validação de mercado**: quem já resolve o problema e o que faria o cliente trocar. Não pule: ela melhora a ideia antes de gastar código.

### 1b. Objeto central e camadas (alimenta `VISAO.md` e `ARQUITETURA.md`)
- Qual é a "coisa" principal de que todo o resto deriva? (Um tema de marca, um pedido, um paciente, um documento...)
- De onde ela nasce (que entrada) e o que é calculado a partir dela? Quem a consome (telas, relatórios, exportações)?
- O que o usuário pode ajustar à mão sem perder o que foi calculado? O que fica guardado como versão ou histórico?
- Existe um motor ou serviço já pronto para reaproveitar (e o que ele **não** deve ser obrigado a reimplementar)?

Daqui saem as **camadas** e o fluxo de dados do relatório do plano. Proponha um desenho e peça confirmação.

### 2. Fluxos (alimenta `NEGOCIO.md`)
- O que a pessoa faz do começo ao fim? (Descreva um dia de uso.)
- Quem são os diferentes tipos de usuário e o que cada um faz?
- Qual o caminho principal e quais os desvios mais comuns?

### 3. Regras (alimenta `NEGOCIO.md`)
Surgem dos fluxos. Para cada passo importante: "E se acontecer X?", "Tem limite, prazo, valor mínimo?", "Quem pode aprovar/cancelar?". Cada regra confirmada vira ✅; as deduzidas, 💡.

### 4. Experiência (alimenta `DESIGN.md`)
- Em que situação usam: celular em movimento, balcão, computador sentado?
- Qual a tarefa principal, em uma frase?
- Algum app ou site de que gostam do jeito ou do visual?
- O cliente já tem logo, cores ou manual de marca? Já tem design system? Em que formato (Figma, código, só logo)?

A partir dos fluxos já levantados, **proponha a lista de telas** e peça confirmação; ninguém deve listar telas do zero. Assuma mobile-first.

### 5. Qualidade (alimenta requisitos não funcionais de `PRODUTO.md`)
- O que não pode falhar nem demorar para o cliente final?
- Lida com dados pessoais, pagamentos ou algo sensível?
- Precisa ser encontrado no Google? (Se sim, cria a seção de SEO.)

Converta as respostas em metas mensuráveis ("o checkout não pode demorar" vira "carregar em menos de 3 s no 4G; checkout em até 3 cliques"). Marque 💡 as metas que você propôs.

### 6. Técnico (alimenta `ARQUITETURA.md`, `CLAUDE.md`)
Só aqui, e geralmente **proposto por você** com base no que foi dito. Pergunte apenas se a equipe tem stack obrigatória ou preferência. Registre: tecnologia, versão, propósito. Pergunte os comandos reais de build/teste/lint quando o projeto já existir.

### 7. Time, agentes, pastas e integrações (nível completo; **perguntar sempre, nunca presumir**)
- Quais agentes fazem sentido **para este projeto**? Proponha com base nas camadas (ex.: um que confere a consistência com o motor externo, um que cuida da saída para o Figma) além de código, design e segurança.
- Como o projeto organiza as pastas? Em projeto existente, mostre "como é hoje" e pergunte o que mudar; em projeto novo, proponha uma estrutura e peça confirmação.
- Persistência e usuários: login, dados por usuário, backup, histórico?
- Quais serviços precisam conectar (GitHub, Jira, Slack, Figma, banco)? Alimenta `.mcp.json`.
- Haverá deploy? Onde? (Se sim, cria `skills/deploy/`.)
- Sistema operacional do time (relevante para os hooks; ver `templates-claude.md`).

## Modos B e C

Nos modos B e C, parte dos blocos 2, 3 e 6 já pode ser levantada lendo o repositório e os documentos antigos (ver `modos-existente.md`). Isso **preenche o rascunho para confirmação**, mas **não dispensa nenhum bloco**: faça todas as perguntas, em especial **negócio** (1), **objeto central e camadas** (1b), **regras** (3), **agentes, pastas e persistência** (7). Comece cada rodada com "li isto no código/documentos: confere?" e siga para o que eles não dizem.

## Fechamento da entrevista

Ao terminar, **não** gere arquivos. Siga para o **relatório do plano** (`relatorio-plano.md`), que traz o resumo (problema, público, diferencial, MVP, camadas, regras, fluxos e telas, stack, o que está em 💡/❓) e tudo o que será criado ou alterado. Ajustes pedidos nesse relatório são baratos; ajustes depois da geração custam retrabalho.
