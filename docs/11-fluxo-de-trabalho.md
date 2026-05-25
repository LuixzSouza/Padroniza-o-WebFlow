# 11 — Fluxo de Trabalho & Governança

> Como a equipe **mantém o padrão vivo**. Um style guide só funciona se houver processo. Este doc é sobre o "como trabalhar junto sem bagunçar".

---

## Sumário

1. [Organização do Style Panel no Webflow](#1-organização-do-style-panel-no-webflow)
2. [Fluxo: criar um componente novo](#2-fluxo-criar-um-componente-novo)
3. [Fluxo: criar uma página nova](#3-fluxo-criar-uma-página-nova)
4. [Revisão de nomenclatura (naming review)](#4-revisão-de-nomenclatura-naming-review)
5. [Decisões em aberto do projeto](#5-decisões-em-aberto-do-projeto)
6. [Regras de convivência](#6-regras-de-convivência)

---

## 1. Organização do Style Panel no Webflow

O Webflow permite **pastas de classes** usando `/` no nome (na verdade as classes ficam achatadas, mas o Designer agrupa visualmente). Organize por namespace:

```
c-     → Componentes   (c-hero, c-card, c-button…)
l-     → Layout         (l-page, l-container, l-grid, l-stack)
u-     → Utilities      (u-text-h1, u-hidden…)
is--   → Modifiers      (is--dark, is--primary…)
```

E as **Variables** em Collections (`Core`/`Cores`/`Spacing`/`Semantic`) conforme [01-tokens](01-tokens.md).

> Quando entrar no Designer, todo mundo deve reconhecer onde cada coisa está só pelo prefixo. Esse é o ganho da padronização.

---

## 1.1. Webflow Components vs. classes

O Webflow tem **Components** (símbolos reutilizáveis — antes "Symbols") com suporte a **Properties** e **Variants**. Eles coexistem com o sistema de classes deste guia:

- **Classes (`c-`/`l-`/`u-`/`is--`)** = a base de estilo de tudo. Sempre presentes.
- **Webflow Component** = embrulha um trecho recorrente (nav, footer, card de post) num bloco editável em um lugar só. Use para o que se repete em **muitas páginas** e precisa atualizar centralizado.
- **Variants** (dentro de um Component) = variações de **estrutura/conteúdo** do Component.

**Regra do projeto:** transforme em Webflow Component os blocos globais (nav, footer, CTA padrão) e os itens de CMS. Mas **a estilização continua via classes** deste guia. Para variações de **estilo**, use **combo class `is--`**, não Variant — detalhe completo da decisão em [08-combo-classes](08-combo-classes.md), seção 1.

> Observação: o turn.io atual já usa Variants (`w-variant-a` no código). No rebuild, padronize: estilo = combo class; estrutura dentro de Component = variant. Não os dois para a mesma coisa.

---

## 2. Fluxo: criar um componente novo

Antes de criar qualquer classe nova, siga o funil:

```
1. Já existe esse componente em 04-componentes.md?
   └─ Sim → use o existente.
   └─ Não ↓

2. É só uma VARIAÇÃO de um componente existente?
   └─ Sim → crie um MODIFIER (combo class is--*), não um Block novo.
   └─ Não ↓

3. É um componente autônomo de verdade (faz sentido sozinho)?
   └─ Sim → crie um Block c-[nome] seguindo a nomenclatura BEM (00).
   └─ Não → provavelmente é um Element de outro Block, ou layout (l-*).

4. Documente em 04-componentes.md (use o template do fim do arquivo).
```

**Checklist ao criar:**

- [ ] Nome segue BEM-Webflow ([00-nomenclatura](00-nomenclatura.md)): `c-`, `__`, `is--`, lowercase, kebab-case
- [ ] Cores via `semantic/*`, spacing via `space/*`, fonte via `u-text-*` (zero valor cru)
- [ ] Modifier é **combo class**, não global duplicada
- [ ] Testado nos 4 breakpoints ([09](09-responsividade.md))
- [ ] Imagens otimizadas ([10](10-performance.md))
- [ ] Adicionado a [04-componentes.md](04-componentes.md)

---

## 3. Fluxo: criar uma página nova

1. Comece pelo esqueleto de layout ([03](03-espacamento-layout.md)): `l-page > c-section > l-container`.
2. Monte com componentes **existentes** da biblioteca.
3. Faltou algo? Siga o fluxo de componente novo (acima).
4. Um único `<h1>` por página; hierarquia de headings correta ([02](02-tipografia.md)).
5. Rode o checklist de performance antes de publicar ([10](10-performance.md)).

---

## 4. Revisão de nomenclatura (naming review)

Como mais de uma pessoa vai mexer, vale um acordo leve de revisão:

- **Antes de publicar em produção**, uma segunda pessoa passa o olho nas classes novas (nome faz sentido? combo correto? não duplicou Block existente?).
- Dúvida de nome → consulta o **Cheat Sheet** do [00-nomenclatura](00-nomenclatura.md) (seção 9) e a tabela de anti-padrões.
- Classe órfã encontrada → "Clean up" no Style Manager ([10](10-performance.md), seção 6).
- Acrônimos novos do projeto → registrar na seção de acrônimos do [00-nomenclatura](00-nomenclatura.md) (regra de ouro #8).

---

## 5. Decisões em aberto do projeto

Registre aqui as escolhas que a equipe ainda precisa bater o martelo (atualize conforme decidir):

| Decisão | Opções | Status |
|---|---|---|
| Nome da escala neutra | `black/*` (atual) vs `neutral/*` (recomendado) | ⏳ a definir |
| Self-host da fonte vs Google Fonts | self-host (mais rápido) vs Google (mais simples) | ⏳ a definir |
| Dark mode | usar Modes de Variables vs não ter | ⏳ a definir |
| Breakpoint de design base | 1200 vs 1440 | ⏳ a definir |

---

## 6. Regras de convivência

1. **Zero valor cru.** Cor, spacing, fonte, radius — sempre via Variable/classe. Quem digitar `#ccc` ou `23px` direto está criando dívida.
2. **Reutilizar > criar.** Toda classe nova é peso de CSS para todos os visitantes. Cheque a biblioteca antes.
3. **Modifier, não duplicação.** Variação = combo class `is--`. Nunca `c-hero-2`.
4. **Documentar é parte do trabalho.** Componente sem entrada em [04](04-componentes.md) não existe para a equipe.
5. **Medir performance** a cada marco ([10](10-performance.md)). Foi a falta disso que trouxe o site ao rebuild.
6. **Em dúvida, pergunte** — é mais barato alinhar nome agora do que renomear classe com interação acoplada depois.

---

**Anterior:** [← 10 — Performance](10-performance.md) | **Voltar ao índice:** [README](../README.md)
