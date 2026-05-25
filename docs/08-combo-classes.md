# 08 — Catálogo de Combo Classes

> Referência mestre de **todas as variações** do projeto. Aqui está o pensamento completo sobre quando e como usar combo de classes — incluindo a decisão entre **Combo Class** e **Webflow Variants** (recurso nativo que o turn.io já usa).

> Pré-requisito: a teoria de combo class está em [00-nomenclatura](00-nomenclatura.md) (seções 5 e 6). Este doc é o **catálogo prático** + o framework de decisão.

---

## Sumário

1. [Combo Class vs. Webflow Variants — qual usar](#1-combo-class-vs-webflow-variants--qual-usar)
2. [Modifiers globais (servem em vários componentes)](#2-modifiers-globais-servem-em-vários-componentes)
3. [Catálogo por componente](#3-catálogo-por-componente)
4. [Ordem e combinação de combos](#4-ordem-e-combinação-de-combos)
5. [Como pensar uma variação nova](#5-como-pensar-uma-variação-nova)
6. [Anti-padrões de combo](#6-anti-padrões-de-combo)

---

## 1. Combo Class vs. Webflow Variants — qual usar

O Webflow tem **dois** mecanismos para variações, e isso confunde. O turn.io atual já usa Variants (vimos `w-variant-a` no código). Decisão clara para o projeto novo:

| Critério | **Combo Class** (`.is--dark`) | **Webflow Variants** (nativo) |
|---|---|---|
| O que é | Segunda classe CSS adicionada sobre a base | Estado/variante definido dentro de um **Component** Webflow |
| Onde vive | Style Panel (classes) | Painel de Componentes |
| Reutilizável fora do componente | ✅ Sim — `.is--dark` serve em vários blocos | ❌ Não — preso ao Component |
| Aparece no HTML | ✅ Sim (`class="c-hero is--dark"`) — legível | Parcial (`w-variant-*`) — menos legível |
| Copy/paste entre projetos | ✅ Mantém o nome | ⚠️ Depende do Component ir junto |
| Melhor para | **Variações de estilo** (cor, tamanho, estado) | **Conteúdo/estrutura diferente** dentro de um Component (ex.: card com/sem imagem) |

### Regra do projeto

- **Padrão = Combo Class (`is--`).** É a nossa convenção principal por ser legível, reutilizável e portável. Tudo neste style guide assume combo class.
- **Use Webflow Variants apenas** quando você já transformou algo num **Webflow Component** (símbolo reutilizável) e a variação muda **estrutura/conteúdo**, não só estilo. Ex.: um Component "Card" com variante "com vídeo" vs "com imagem".
- **Nunca** misture os dois para a mesma coisa (não crie `is--dark` E uma variant "Dark" — escolha um). Para cor/tamanho/estado → sempre combo class.

> Resumindo: **estilo → combo class. Estrutura dentro de Component → variant.**

---

## 2. Modifiers globais (servem em vários componentes)

Estes `is--` se repetem em muitos lugares. Padronize o **nome e o significado** para que `is--dark` queira dizer a mesma coisa em qualquer componente.

### Cor / tema

| Combo | Significado | Aplica em |
|---|---|---|
| `is--dark` | Fundo escuro / tema escuro de marca (purple/5) | section, nav, hero, cta, card |
| `is--light` | Fundo claro explícito (quando a base é escura) | section, nav |
| `is--brand` | Cor de marca no texto/elemento | text, badge, icon |
| `is--inverse` | Texto/ícone claro para fundo escuro | text, link, richtext |
| `is--muted` | Versão apagada (texto secundário) | text, link |

> **Variantes por família de cor:** como o turn.io tem 8 famílias (purple, blue, green, pink, red, yellow, neutral), componentes "coloríveis" (badge, alert, section, icon) podem ter um set padronizado: `is--purple`, `is--blue`, `is--green`, `is--pink`, `is--red`, `is--yellow`, `is--neutral`. Use o **mesmo conjunto de nomes** em todos eles.

### Estado

| Combo | Significado |
|---|---|
| `is--active` | Item atual / selecionado (link de nav, tab, paginação) |
| `is--disabled` | Desabilitado (opacidade + sem ponteiro) |
| `is--loading` | Carregando (spinner / texto oculto) |
| `is--open` | Aberto (accordion, dropdown, modal, menu mobile) |
| `is--error` | Estado de erro (input, badge, alert) |
| `is--success` | Estado de sucesso |

### Tamanho

| Combo | Significado |
|---|---|
| `is--xs` / `is--small` | Menor |
| `is--large` / `is--xl` | Maior |
| `is--compact` | Menos padding/respiro |
| `is--spacious` | Mais respiro |
| `is--full` | Largura 100% |

### Alinhamento / layout

| Combo | Significado |
|---|---|
| `is--center` | Centralizado (texto + itens) |
| `is--left` / `is--right` | Alinhamento explícito |
| `is--reverse` | Inverte ordem (ex.: hero com mídia à esquerda) |
| `is--horizontal` / `is--vertical` | Direção (card, divider, stack) |

### Padronização de estados disabled/loading (aplicável a botões, links)

```css
/* Reaproveitável: aplique a base + is--disabled */
.is--disabled {
  opacity: 0.5;
  pointer-events: none;
  cursor: not-allowed;
}
```

> Cuidado: `is--disabled` como **modifier solto** só funciona se você aceitar que ele é um utilitário de estado. Se quiser rigor BEM, prefira `.c-button.is--disabled` (combo na base). Documente a escolha em [11](11-fluxo-de-trabalho.md).

---

## 3. Catálogo por componente

Lista completa dos combos definidos em cada componente. (Definição CSS está no doc indicado.)

### Layout — [03](03-espacamento-layout.md)
- `c-section`: `is--alt` `is--dark` `is--compact` `is--spacious` `is--flush-top` `is--flush-bottom`
- `l-container`: `is--narrow` `is--wide` `is--full`
- `l-stack`: `is--xs` `is--sm` `is--lg` `is--xl` `is--center`
- `l-grid`: `is--2col` `is--4col` `is--auto`

### Tipografia — [02](02-tipografia.md)
- `u-text-*`: `is--center` `is--muted` `is--brand` `is--inverse` `is--measure`

### Componentes — [04](04-componentes.md)
- `c-button`: `is--primary` `is--secondary` `is--ghost` `is--large` `is--small` `is--full` · **(adicionar)** `is--icon-only` `is--loading` `is--white`
- `c-nav`: `is--transparent` `is--dark` `is--sticky` · `c-nav__link.is--active`
- `c-hero`: `is--centered` `is--dark` `is--compact` · **(adicionar)** `is--reverse`
- `c-card`: `is--featured` `is--compact` `is--horizontal`
- `c-cta`: `is--left`
- `c-badge`: `is--success` `is--error` `is--warning` `is--neutral` (+ família de cor)
- `c-link`: `is--arrow` `is--inverse`
- `c-pricing`: `is--featured`
- `c-avatar`: `is--sm` `is--lg`
- `c-divider`: `is--vertical`
- `c-alert`: `is--success` `is--error` `is--warning`
- `c-stats`, `c-tabs`, `c-accordion`, `c-dropdown`, `c-modal`: estados via `is--open`/`is--active`

### Formulários — [05](05-formularios.md)
- `c-field`: `is--inline`
- `c-form`: `is--full`
- `c-input`/`c-textarea`: `is--error`

### CMS — [07](07-cms-richtext.md)
- `c-richtext`: `is--narrow` `is--inverse`

---

## 4. Ordem e combinação de combos

No Webflow você pode empilhar **mais de um** combo. A ordem em que aparecem importa para especificidade.

```html
<!-- base + cor + tamanho + largura -->
<a class="c-button is--primary is--large is--full">Começar</a>

<!-- section: cor + respiro -->
<section class="c-section is--dark is--spacious">

<!-- hero: layout + cor -->
<div class="c-hero is--reverse is--dark">
```

**Regras:**
1. A **base sempre primeiro** (`c-button`), depois os combos.
2. No Webflow, ao criar a combinação, selecione a base + adicione os combos **na ordem em que quer que apareçam**. O Designer mostra a combo como "Base + A + B".
3. Combos do mesmo "eixo" não se misturam: não use `is--primary is--ghost` juntos (conflito). Eixos diferentes (cor + tamanho) sim.
4. Estilize cada combo só com **o que ele muda** — nunca redefina a base inteira.

---

## 5. Como pensar uma variação nova

Fluxo mental antes de criar um combo novo:

```
Preciso de uma variação de um componente existente?
│
├─ É só estilo (cor, tamanho, espaçamento, estado)?
│    └─ SIM → Combo Class is--[nome]. Reutilize um nome global se existir
│             (is--dark, is--large…). Estilize só o que difere.
│
├─ Muda a ESTRUTURA/conteúdo e o componente já é um Webflow Component?
│    └─ SIM → Webflow Variant.
│
└─ É um componente totalmente diferente?
     └─ Não é combo — crie um Block c-novo ([04] template).
```

Perguntas de sanidade:
- O nome do combo já existe em outro componente com **outro** significado? Se sim, escolha outro nome (consistência > criatividade).
- A variação vai ser usada **mais de uma vez**? Se for única e pontual, talvez seja um ajuste de utilitário (`u-*`), não um combo.

---

## 6. Anti-padrões de combo

| ❌ Errado | ✅ Certo | Por quê |
|---|---|---|
| `.c-hero-dark` (global nova) | `.c-hero.is--dark` (combo) | Global duplica a base inteira |
| `.is--dark` sozinha num elemento | `.c-section.is--dark` | Modifier exige classe base |
| `.c-card.is--featured.is--compact.is--horizontal.is--purple.is--shadow` | Reavalie — vira componente novo | Combo demais = componente disfarçado |
| `is--primary` significando "azul" no botão e "destaque" no badge | Mesmo nome = mesmo conceito | Inconsistência confunde a equipe |
| `is--big` no botão e `is--large` no card | Padronize: sempre `is--large` | Sinônimos espalham a convenção |
| Combo + Webflow Variant para a mesma variação | Escolha **um** mecanismo | Duplicação de fonte da verdade |
| Editar a base com o combo selecionado por engano | Confira o seletor ativo no Designer | Quebra todos os usos da base |

> **Alerta Webflow:** ao estilizar uma combo, confirme no topo do Style Panel que está editando `c-hero + is--dark` e **não** só `c-hero`. Editar a base sem querer é o erro nº 1 e afeta o site inteiro.

---

**Anterior:** [← 07 — CMS & Rich Text](07-cms-richtext.md) | **Próximo:** [09 — Responsividade →](09-responsividade.md)
