# Seções avançadas dos documentos

Complementam `templates-docs.md`. **Cada seção é condicional ao perfil** (`perfis.md`) e ao que a entrevista (`entrevista-aprofundada.md`) apurou. Seção que não se aplica é removida (e, se foi avaliada, registrada como "não se aplica" no relatório do plano). Seção aplicável sem informação fica com **uma linha ❓**. Marque a certeza ✅/💡/❓ e use IDs de `perfis.md`.

## Sumário
1. PRODUTO.md: módulos, catálogo de funcionalidades, objetivos, monetização
2. NEGOCIO.md: permissões, transições, dados
3. ARQUITETURA.md: integrações, segurança, ambientes, migrações
4. DESIGN.md: navegação, estados da interface, patterns e templates, tokens
5. DECISOES.md: status da decisão

---

## 1. PRODUTO.md

### 1.1 Módulos e catálogo de funcionalidades (substitui a lista simples da seção 6 a partir do perfil 2)
Agrupe por módulo; cada módulo é um domínio com responsabilidade própria.

```markdown
## 6. Módulos e funcionalidades
### Módulo: <nome> (ex.: Acesso, Operação, Relatórios, Configurações)
| ID | Funcionalidade | Ator | Prioridade | Regras (RN) | Telas (SCR) | Certeza |
|---|---|---|---|---|---|---|
| FEAT-01 | | | MVP | RN-01 | SCR-01 | |

Prioridade: **MVP** (necessário para entregar a proposta de valor) · **Pós-MVP** (importante, pode esperar) · **Futuro** (possibilidade) · **Fora de escopo** (não será feito nesta versão).
```

Ficha por funcionalidade **só** para as de MVP que tenham fluxo crítico, regra ambígua ou vários atores (as demais ficam na linha da tabela):

```markdown
### FEAT-01: <nome>
- **Objetivo:** que necessidade atende.
- **Ator e permissão:** quem pode usar.
- **Pré-condições:** o que precisa existir antes.
- **Fluxo principal:** passos do caminho esperado.
- **Fluxos alternativos e exceções:** outros caminhos; como reage a erros.
- **Dados envolvidos:** informações necessárias (E-nn).
- **Estados:** situações possíveis (ver transições em NEGOCIO.md).
- **Critérios de aceite:** Dado... quando... então... (observáveis).
```

Critério de conclusão do catálogo: cada funcionalidade prioritária pode ser implementada e testada sem depender de interpretação importante ainda ❓.

### 1.2 Objetivos (substitui a seção 8 a partir do perfil 2)
Separe três níveis, para ligar o que se constrói ao motivo:

| Nível | Pergunta | Exemplo |
|---|---|---|
| Negócio | Que resultado justifica o investimento? | Reduzir custo operacional |
| Usuário | Que benefício a pessoa espera? | Fazer a tarefa com menos esforço |
| Funcional | Que comportamento do sistema produz isso? | Automatizar a tarefa repetitiva (FEAT-04) |
| Indicador | Como saber que funcionou? | Tempo médio da tarefa de X para Y (❓ se sem dados) |

Não invente metas numéricas sem dados: registre "meta a definir" (❓).

### 1.3 Monetização e legal (só se houver cobrança ou dados pessoais)
```markdown
## Monetização
<Modelo (pago, grátis, assinatura, uso interno), planos, limites de uso, período de teste, o que acontece no vencimento.> ✅/💡/❓

## Requisitos legais e privacidade
<Dados pessoais coletados e finalidade, base legal/consentimento, retenção, exclusão a pedido (LGPD), termos e política de privacidade necessários.> ✅/💡/❓
```

---

## 2. NEGOCIO.md

### 2.1 Papéis e matriz de permissões (perfil 2 e 3)
```markdown
## 4. Papéis e permissões
| Papel | Objetivo | Escopo (tudo / organização / unidade) | Certeza |
|---|---|---|---|

### Matriz (✔ pode · ✖ não pode · ◐ só o que é seu / com condição)
| Ação (FEAT) | Admin | Gestor | Operador | Cliente |
|---|---|---|---|---|
| FEAT-01 Cadastrar | ✔ | ✔ | ◐ | ✖ |
```
Mantenha **a regra de acesso no servidor**, não só na interface: registre em `ARQUITETURA.md` onde ela é aplicada.

### 2.2 Várias empresas (só perfil 3)
```markdown
## 5. Organizações (multi-tenant)
- **Como uma organização é identificada:** 
- **Usuário em várias organizações:** sim/não; como troca de contexto.
- **Unidades/filiais:** 
- **Isolamento:** todo registro pertence a uma organização (campo e regra de consulta); acesso cruzado é impedido no servidor/banco, nunca só na tela.
- **Administrador da plataforma:** o que vê e faz; como é auditado.
- **Planos e limites:** relação entre plano, assinatura e funcionalidades liberadas.
```

### 2.3 Ciclo de vida e transições (para cada entidade com status)
```markdown
## Ciclo de vida: <Entidade E-nn>
| De | Para | Quem pode | Condição | Efeito / observação | RN |
|---|---|---|---|---|---|
| rascunho | pendente | Operador | campos obrigatórios completos | notifica gestor | RN-04 |
Transições não listadas são **proibidas** (cada proibição relevante vira RN).
```

### 2.4 Dados (só se houver banco; fica aqui, não em documento à parte)
```markdown
## Modelo de dados
**Relações:** <Organização 1:N Usuário; Pedido 1:N Item; N:N via entidade associativa X> (cardinalidade e opcionalidade).

### Dicionário de dados: <Entidade E-nn>  (finalidade: <por que existe>)
| Campo | Tipo | Obrig. | Chave | Padrão | Validação | Sensível | Finalidade |
|---|---|---|---|---|---|---|---|

**Únicos e índices:** <o que não pode repetir; consultas frequentes que pedem índice.>

## Regras de persistência
| Entidade | Criação | Alteração permitida | Exclusão (física / lógica / arquivar) | Histórico | Idempotência |
|---|---|---|---|---|---|
Decida **por entidade**; não aplique exclusão lógica a tudo por padrão. Dados que precisam ser preservados, anonimizados ou removidos a pedido ficam explícitos aqui.
```
Perfil 2: dicionário resumido (campos principais e sensíveis). Perfil 3: dicionário completo. O modelo físico (tipos exatos, índices) pode nascer na tarefa de fundação de dados, com a spec.

---

## 3. ARQUITETURA.md

### 3.1 Inventário de integrações (substitui a seção 4 a partir do perfil 2)
```markdown
## 4. Integrações externas
### INT-01: <serviço>
| Item | Detalhe |
|---|---|
| Finalidade | |
| Fornecedor | |
| Dados enviados / recebidos | |
| Autenticação | <API key, OAuth...> (valores **nunca** aqui; só os nomes das variáveis de ambiente) |
| Quem inicia a comunicação | |
| Se o serviço falhar | <comportamento alternativo, fila, nova tentativa> |
| Limites e custos | <cotas, preço por uso> |
| Duplicidade | <como evitar processar o mesmo evento duas vezes> |
| Ambiente de teste | <sandbox, mock> |
| Certeza | |
```

### 3.2 Segurança e confiabilidade (proporcional ao risco do bloco E)
```markdown
## Segurança e confiabilidade
**Nível de risco:** baixo / médio / alto (✅/💡)
| Tema | Decisão | Certeza |
|---|---|---|
| Autenticação e sessão | | |
| Autorização (onde é aplicada) | | |
| Validação de entradas | cliente e servidor | |
| Segredos | variáveis de ambiente; fora do repositório | |
| Dados pessoais e logs | o que não pode ir para log | |
| Auditoria | quais operações são registradas | |
| Limites de requisição | | |
| Backup e restauração | frequência; restauração testada? | |
| Dependências | como são atualizadas | |
```
Só inclua as linhas que o nível de risco exige.

### 3.3 Ambientes, publicação e migrações
```markdown
## Ambientes e publicação
| Ambiente | Onde roda | Banco | Quem publica | Como |
|---|---|---|---|---|
| Local | | | | |
| Produção | | | | |
**Rollback:** <como voltar uma publicação com problema>. **Monitoramento:** <logs, erros, alertas>.

## Migrações do banco (se houver)
Ferramenta e versionamento do esquema; ordem de execução; dados iniciais e de exemplo (seeds); estratégia de reversão; backup antes de mudança crítica. Alterações manuais no banco não são permitidas (divergem do código versionado).
```

---

## 4. DESIGN.md

### 4.1 Navegação e arquitetura da informação (acima do mapa de telas)
```markdown
## 3b. Navegação
<Árvore das seções e telas; menus e submenus; o que muda por papel; onde ficam configurações.>
```
```text
App
├─ Painel
├─ <Módulo>
│  ├─ Lista (SCR-01) → Detalhe (SCR-02) → Cadastro/edição (SCR-03)
└─ Configurações (Perfil, Organização, Usuários)
```

### 4.2 Estados da interface (por tela ou por tipo de tela)
Complementa "vazio, carregando, erro, sucesso" com os casos que costumam ser esquecidos:

| Estado | Pergunta |
|---|---|
| Vazio | Ainda não existe nada: que ação recomendar? |
| Sem resultados | Existem dados, mas o filtro não casou: como limpar? (diferente do vazio) |
| Carregando | Inicial ou parcial? |
| Erro de comunicação | Tentar de novo? Dados parciais? |
| Erro de validação | Mensagem por campo, em linguagem clara |
| Sem permissão | O que a pessoa vê? |
| Sessão expirada | Volta para onde depois de entrar? |
| Operação em andamento / indisponível | O que bloqueia e o que continua? |
| Feedback de ação | Salvar, excluir, enviar, cancelar |

Na ficha da tela (opcional), liste só os estados relevantes para ela.

### 4.3 Responsividade e acessibilidade
Breakpoints; o que muda de desktop para celular (tabela vira cartões, menu lateral vira inferior); modais em tela pequena; meta de acessibilidade (ex.: WCAG 2.1 AA ✅/💡): teclado, foco visível, contraste, rótulos de formulário, mensagens de erro claras, não depender só de cor.

### 4.4 Patterns e templates (perfil 2 e 3, quando há mais de 3 telas parecidas)
```markdown
## Patterns e templates
**Patterns** (soluções recorrentes): cadastro em etapas · filtros de lista · busca com resultados · confirmação de exclusão · formulário inválido · estado vazio com ação · <outros do projeto>
**Templates** (estruturas de página): painel · lista · detalhe · formulário · configurações · autenticação
Para cada um: quando usar, que componentes compõe, qual tela (SCR-nn) o utiliza.
```
Organização sugerida da documentação do design system: **Foundations** (fundamentos e tokens) → **Components** → **Patterns** → **Templates**.

### 4.5 Tokens em camadas
Nomeie com convenção consistente e separe três níveis, para trocar marca ou tema sem refazer telas:

| Nível | Papel | Exemplo |
|---|---|---|
| Primitivo | Valor bruto | `orange.500` |
| Semântico | Intenção de uso | `color.action.primary` |
| Componente | Aplicação | `button.background.primary` |

Categorias: cor, tipografia, espaçamento, dimensões, bordas/raios, sombras, opacidade, movimento (duração e curva; respeitar movimento reduzido), camadas (z-index). Os nomes acima são exemplos de organização, não padrão obrigatório.

---

## 5. DECISOES.md

Acrescente o **status** a cada entrada, para uma sugestão nunca virar decisão sem alguém perceber:

| Status | Significado |
|---|---|
| ✅ Confirmada | Validada pelo usuário |
| 💡 Proposta | Recomendação ainda não aprovada |
| ❓ Pendente | Falta informação para decidir; diga o que bloqueia |
| 🚫 Descartada | Alternativa avaliada e rejeitada, com o motivo |

Registre aqui também: o perfil de profundidade escolhido (e mudanças dele), a estratégia de Git e a hipótese de "uma tarefa por sessão" (ver `tarefas-e-fases.md`).
