# Entrevista aprofundada (blocos A a G)

Complementa `entrevista.md`. Cada bloco só entra se o **perfil** (`perfis.md`) pedir, e só as perguntas que fazem sentido para o projeto. Mesma conduta de sempre: extrair antes de perguntar, rodadas curtas, linguagem de negócio, hipótese quando a pessoa não souber, certeza ✅/💡/❓ em cada resposta.

Cada bloco diz **para onde a resposta vai**. As respostas viram conteúdo das seções avançadas descritas em `templates-avancados.md`.

## A. Papéis e permissões (perfil 2 e 3; alimenta `NEGOCIO.md`)
- Que tipos de pessoa usam isso e o que cada uma pode fazer? (Quem administra, quem opera, quem só consulta, quem é o cliente final.)
- Quem compra é a mesma pessoa que usa?
- Existem coisas que só um papel pode fazer? E coisas que um papel nunca pode ver?
- Um papel pode valer só para uma parte (uma unidade, uma equipe, uma loja) ou vale para tudo?
- Os papéis são fixos ou o cliente poderá criar os próprios?

Resultado: **matriz de permissões** (papel × ação por módulo). Distinga perfil de usuário de permissão técnica: dois gestores podem ter alcances diferentes.

## B. Várias empresas e planos (perfil 3; alimenta `NEGOCIO.md` e `ARQUITETURA.md`)
- O sistema atende mais de uma empresa/cliente, cada uma com os próprios dados? Elas nunca podem enxergar os dados umas das outras?
- Uma pessoa pode pertencer a mais de uma empresa? Como troca entre elas?
- Existem unidades, filiais ou departamentos dentro de uma empresa?
- Existe um administrador da plataforma (quem opera o sistema) com poderes especiais? O que ele pode ver?
- Há planos, limites de uso, período de teste, cobrança recorrente? O que acontece quando o plano vence?

Pergunte **mesmo que o primeiro cliente seja um só**: decisão estrutural errada aqui é cara de corrigir depois. Se a resposta for "ainda não sei", registre ❓ e marque que a tarefa de dados e autenticação fica bloqueada até decidir.

## C. Ciclo de vida das coisas (perfil 2 e 3; alimenta `NEGOCIO.md`)
Para cada entidade que muda de situação (pedido, assinatura, solicitação, tarefa, documento):
- Quais situações ela pode ter, em ordem? (Rascunho, pendente, em andamento, concluído, cancelado...)
- Quem pode mudar de uma para outra, e sob que condição?
- Dá para voltar atrás? O que acontece com o que já foi feito?
- O que o sistema deve fazer quando alguém tenta uma mudança não permitida?

Resultado: **tabela de transições** (de → para, quem, condição, o que acontece). Cada transição proibida vira RN.

## D. Integrações (perfil 2 e 3; alimenta `ARQUITETURA.md`)
Para cada serviço externo (pagamento, e-mail, WhatsApp, login social, mapas, IA, ERP, armazenamento...):
- Para que serve e qual fornecedor (ou ❓)?
- Que dados saem e entram?
- O que o sistema faz se o serviço ficar fora do ar?
- Tem custo por uso ou limite de chamadas?
- Existe ambiente de teste do fornecedor?
- A mesma notificação pode chegar duas vezes (ex.: pagamento)? Como evitar cobrar ou atualizar em duplicidade?

Resultado: **inventário de integrações** (`INT-nn`). Credenciais nunca são pedidas nem gravadas: só se anota quais variáveis de ambiente serão necessárias.

## E. Segurança, privacidade e legal (proporcional ao risco; alimenta `ARQUITETURA.md` e `PRODUTO.md`)
Primeiro classifique o risco, em uma pergunta: "O sistema guarda dados pessoais, financeiros ou sensíveis de outras pessoas?"
- **Baixo** (conteúdo público): validação de entradas e segredos fora do código bastam.
- **Médio** (login e dados pessoais comuns): + autenticação e autorização, política de logs, backup, LGPD básica (finalidade, consentimento, exclusão a pedido).
- **Alto** (financeiro, saúde, dados sensíveis, várias empresas): + auditoria de operações relevantes, limites de requisição, retenção definida, recuperação de falhas testada.

Perguntas: quais dados pessoais são coletados e para quê? Por quanto tempo guardar? A pessoa pode pedir exclusão? Quem pode ver o quê? Precisa de registro de quem fez o quê?

## F. Dados e persistência (só se houver banco; alimenta `NEGOCIO.md`)
- Que "coisas" o sistema guarda? (Proponha a lista de entidades a partir dos fluxos e confirme.)
- Como elas se relacionam? (Um cliente tem vários pedidos? Um pedido tem vários itens?)
- Algum dado é sensível? Algum precisa de histórico de alterações?
- Quando algo é excluído, some de verdade, fica escondido ou fica arquivado? Pode haver exclusão se existirem dependências?
- Existem dados duplicáveis que precisam ser evitados (e-mail único, documento único)?
- Quais consultas serão mais frequentes e qual o volume esperado?
- Há dados iniciais necessários (categorias, planos, papéis)?

Resultado: modelo de domínio (`E-nn`), **dicionário de dados**, regras de persistência. Não aplique exclusão lógica a tudo por padrão: decida por entidade.

## G. Ambientes, operação e ferramentas (perfil 2 e 3; alimenta `ARQUITETURA.md` e `CLAUDE.md`)
- Quantos ambientes (local, homologação, produção)? Onde cada um roda?
- Como publica hoje ou como pretende publicar? Alguém precisa aprovar?
- O que acontece se uma publicação der errado? Existe como voltar?
- Quem acompanha erros e uso (logs, alertas)?
- Há backup? Já foi testado restaurar?
- Estratégia de branches (ver `perfis.md`) e convenção de commits.
- Ferramentas de qualidade já usadas: lint, formatação, testes, CI.

## Design system (quando há interface; alimenta `DESIGN.md`)
Ver `design-system.md`. A pergunta de entrada é "já existe design system ou manual de marca?". Se **não existe**, use as perguntas guia da seção "Quando não há nada" daquele arquivo, em rodadas curtas, propondo valores a partir do que a pessoa disser sobre o público e o tom.
