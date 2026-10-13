# Design system e design: quatro caminhos

Aplique quando o projeto tem interface. O design system é uma **decisão com caminhos**, não um arquivo fixo. A fonte oficial dos valores é o código (tokens do Tailwind) ou o Figma; a seção "Design system" do `DESIGN.md` mostra tudo de forma legível e registra a **origem**. Quando o design system já existe fora do repositório, o arquivo só aponta para ele.

## Pergunta de entrada

"O cliente já tem design system ou manual de marca? Em que formato: Figma, código, ou só logo e cores?" A resposta escolhe o caminho.

## Os quatro caminhos

| Caminho | Situação | Procedimento |
|---|---|---|
| **A. Figma** | Já existe no Figma | Peça o link do arquivo ou da biblioteca. Use as ferramentas do MCP do Figma disponíveis (variáveis, busca no design system, contexto de design) para ler variáveis, estilos e componentes. Registre tokens e a lista de componentes. Se o arquivo não tiver variáveis, **avise** e levante só o que for confiável, marcando o resto 💡/❓; não invente valores. Depende de a conta conectada ter permissão. |
| **B. Código** | Já existe pacote, Storybook ou tokens no repositório | Aponte para o pacote ou caminho, resuma tokens e componentes disponíveis, e registre a origem. |
| **C. Logo e identidade** | Só há logo e cores | Use a ferramenta de geração de design system a partir do logo que a equipe desenvolve, se estiver disponível, e importe o resultado (tokens e código). Se não estiver, caia no caminho D partindo das cores do logo, marcadas 💡. |
| **D. Nada** | Não existe nada | Configuração mínima: paleta com papéis (primária, secundária, sucesso, alerta, erro, fundo), uma família tipográfica com escala de tamanhos, escala de espaçamento e componentes base. Implemente como tokens do Tailwind. |

Todos os caminhos terminam igual: **tokens no código** + a seção "Design system" do `DESIGN.md` com origem, onde estão os tokens, componentes disponíveis e a regra de ouro: **usar apenas tokens e componentes existentes; nada de cor ou espaçamento avulso.**

## Tailwind como base

Verifique a versão do Tailwind no projeto: na v3 os tokens ficam em `tailwind.config`; na v4, no bloco `@theme` do CSS. Se o projeto não usa Tailwind e a equipe não exigir outra coisa, proponha como padrão (💡).

## Mobile-first

- Projetar a partir de ~360 px e ampliar com os prefixos `md:` e `lg:`.
- Áreas de toque de pelo menos 44 px; navegação ao alcance do polegar; nada que dependa de *hover*.
- Contraste, tamanho mínimo de fonte e foco visível (acessibilidade básica).
- Cada tela prevê os estados: vazio, carregando, erro e sucesso.

## O que o `DESIGN.md` guarda

1. Princípios de UX e contexto de uso (quem, onde, em que situação).
2. Regras mobile-first.
3. **Mapa de telas** com ID, nome, fluxo, prioridade, tarefa e status (ver template).
4. Design system: origem, tokens, componentes.
5. Referências: links do Figma, apps de referência, identidade da marca.

A lista de telas é **proposta pela skill** a partir dos fluxos e confirmada pelo usuário. Na fase de telas do `TASKS.md`, a tarefa de fundação visual (x.0) vem antes de qualquer tela, e as telas são agrupadas por fluxo.
