# 03 — Espaçamento & Layout

> O esqueleto que todo componente vive dentro. Namespace de layout `l-` (ver [00-nomenclatura](00-nomenclatura.md), apêndice).
> **Cada classe vem com o CSS completo pronto para recriar no Webflow.**

---

## Sumário

1. [A hierarquia de layout](#1-a-hierarquia-de-layout)
2. [l-page](#2-l-page)
3. [c-section](#3-c-section)
4. [l-container](#4-l-container)
5. [l-stack (empilhamento vertical)](#5-l-stack-empilhamento-vertical)
6. [l-grid](#6-l-grid)
7. [Exemplo de página completa](#7-exemplo-de-página-completa)
8. [Regras de ouro](#8-regras-de-ouro)

---

## 1. A hierarquia de layout

```
l-page                      ← wrapper de toda a página (overflow)
  └── c-section             ← faixa horizontal (fundo + padding vertical)
        └── l-container     ← largura máxima + padding horizontal
              └── l-stack / l-grid   ← organiza os filhos
                    └── [componentes]   ← c-hero, c-card, c-button…
```

Cada nível tem **uma** responsabilidade. Decorar isso resolve 90% da bagunça de layout.

> **Lembrete sobre valores:** tudo que é cor/spacing usa **Variables** (ver [01-tokens](01-tokens.md)). Abaixo escrevo tanto a Variable (`var(--space-md)`) quanto o valor em px entre comentários, para você saber o que digitar no Webflow.

---

## 2. l-page

Envolve tudo dentro do `<body>`. Contém scroll horizontal acidental.

```css
.l-page {
  position: relative;
  width: 100%;
  overflow-x: hidden;
}
```

No Webflow: aplique no primeiro `Div Block` logo dentro do Body, que envolve a página inteira.

---

## 3. c-section

A faixa horizontal. Controla **padding vertical** e **cor de fundo** — nada mais. Largura é sempre 100%.

```css
.c-section {
  position: relative;
  width: 100%;
  padding-top: var(--space-3xl);     /* 96px */
  padding-bottom: var(--space-3xl);  /* 96px */
  background-color: var(--semantic-surface);  /* branco */
}
```

### Modifiers (combo classes)

```css
/* Fundo alternado (cinza claro) */
.c-section.is--alt {
  background-color: var(--semantic-surface-alt);  /* neutral/1 #f2f2f2 */
}

/* Fundo escuro de marca + texto claro */
.c-section.is--dark {
  background-color: var(--color-purple-5);   /* #460095 */
  color: var(--semantic-text-inverse);       /* branco */
}

/* Menos respiro vertical */
.c-section.is--compact {
  padding-top: var(--space-xl);      /* 48px */
  padding-bottom: var(--space-xl);   /* 48px */
}

/* Mais respiro (seções de destaque) */
.c-section.is--spacious {
  padding-top: var(--space-4xl);     /* 128px */
  padding-bottom: var(--space-4xl);  /* 128px */
}

/* Cola no topo (sem padding em cima — ex.: primeira seção após nav) */
.c-section.is--flush-top {
  padding-top: 0;
}

.c-section.is--flush-bottom {
  padding-bottom: 0;
}
```

> **Regra:** padding **vertical** mora SEMPRE aqui, nunca no componente. Ritmo vertical da página controlado num só lugar.

### Responsivo (Tablet/Mobile)

Como o padding usa `var(--space-3xl)`, dá pra deixar a Variable fluida com `clamp()`. Senão, reduza no breakpoint Tablet:

```css
/* @ Tablet (≤991px) */
.c-section { padding-top: var(--space-2xl); padding-bottom: var(--space-2xl); }  /* 64px */
```

---

## 4. l-container

Limita largura e centraliza. Aplica o **padding horizontal** (gutter lateral).

```css
.l-container {
  width: 100%;
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
  padding-left: var(--space-sm);   /* 16px gutter */
  padding-right: var(--space-sm);  /* 16px gutter */
}
```

### Variações de largura

```css
/* Texto longo, blog, páginas legais (~65 caracteres por linha) */
.l-container.is--narrow {
  max-width: 760px;
}

/* Galerias, grids largos */
.l-container.is--wide {
  max-width: 1400px;
}

/* Full-bleed (sem limite) */
.l-container.is--full {
  max-width: none;
}
```

### Responsivo

```css
/* @ Mobile Landscape (≤767px) — gutter um pouco maior fica melhor */
.l-container { padding-left: var(--space-md); padding-right: var(--space-md); }  /* 24px */
```

---

## 5. l-stack (empilhamento vertical)

Empilha filhos na vertical com `gap` consistente. Use em qualquer grupo (título + subtítulo + botão).

```css
.l-stack {
  display: flex;
  flex-direction: column;
  align-items: flex-start;     /* itens alinhados à esquerda */
  gap: var(--space-md);        /* 24px entre filhos */
}
```

### Modifiers

```css
.l-stack.is--xs   { gap: var(--space-2xs); }  /* 8px  */
.l-stack.is--sm   { gap: var(--space-sm); }   /* 16px */
.l-stack.is--lg   { gap: var(--space-lg); }   /* 32px */
.l-stack.is--xl   { gap: var(--space-xl); }   /* 48px */

/* Centralizar conteúdo (hero centralizado, CTA) */
.l-stack.is--center {
  align-items: center;
  text-align: center;
}
```

> `gap` substitui margens entre elementos — sem margens órfãs, sem margin collapse.

---

## 6. l-grid

```css
.l-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--space-lg);   /* 32px */
}
```

### Modifiers

```css
.l-grid.is--2col { grid-template-columns: repeat(2, 1fr); }
.l-grid.is--4col { grid-template-columns: repeat(4, 1fr); }

/* Quebra automática: cada coluna tem no mínimo 280px, o resto se ajusta sozinho */
.l-grid.is--auto {
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
}
```

### Responsivo (importante)

Grids precisam virar 1 coluna no mobile. `.is--auto` já se vira sozinho; os de coluna fixa precisam de override:

```css
/* @ Tablet (≤991px) */
.l-grid.is--4col { grid-template-columns: repeat(2, 1fr); }

/* @ Mobile Landscape (≤767px) */
.l-grid,
.l-grid.is--2col,
.l-grid.is--4col {
  grid-template-columns: 1fr;
}
```

No Webflow, você muda `grid-template-columns` em cada breakpoint pelo painel de Grid.

---

## 7. Exemplo de página completa

```html
<div class="l-page">

  <!-- Faixa 1: hero, fundo alternado -->
  <section class="c-section is--alt is--flush-top">
    <div class="l-container">
      <div class="c-hero">...</div>
    </div>
  </section>

  <!-- Faixa 2: grid de cards -->
  <section class="c-section">
    <div class="l-container">
      <div class="l-stack is--center">
        <h2 class="u-text-h2">Recursos</h2>
        <p class="u-text-body-lg is--muted is--measure">Subtítulo da seção.</p>
      </div>
      <div class="l-grid is--auto">
        <div class="c-card">...</div>
        <div class="c-card">...</div>
        <div class="c-card">...</div>
      </div>
    </div>
  </section>

  <!-- Faixa 3: CTA escuro -->
  <section class="c-section is--dark">
    <div class="l-container is--narrow">
      <div class="c-cta">...</div>
    </div>
  </section>

</div>
```

---

## 8. Regras de ouro

1. **Padding vertical → `c-section`.** Padding horizontal → `l-container`. Nunca duplique.
2. **Largura máxima → só no `l-container`.** Componentes ocupam 100% do que recebem.
3. **Espaço entre filhos → `gap`** (`l-stack`/`l-grid`), não margens soltas.
4. Todo spacing vem de `space/*`. Nada de `padding: 47px`.
5. Componente **nunca** define a própria margem externa — quem posiciona é o pai.

---

**Anterior:** [← 02 — Tipografia](02-tipografia.md) | **Próximo:** [04 — Componentes →](04-componentes.md)
