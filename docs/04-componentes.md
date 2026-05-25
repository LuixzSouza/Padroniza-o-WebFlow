# 04 — Biblioteca de Componentes

> Inventário dos componentes do site. **Cada classe (Block, Element e Modifier) vem com o CSS completo pronto para recriar no Webflow.** Esta é a "fonte da verdade" — antes de criar um componente novo, confira se já não existe aqui.

Nomenclatura segue [00-nomenclatura](00-nomenclatura.md) (Block `c-`, Element `__`, Modifier combo `is--`). Cores via `var(--semantic-*)` / `var(--color-*)`, spacing via `var(--space-*)`, radius via `var(--radius-*)` (ver [01-tokens](01-tokens.md)).

---

## Sumário

- [Como recriar no Webflow](#como-recriar-no-webflow)

**Estruturais / navegação:** [c-nav](#c-nav) (+ mega-menu) · [c-footer](#c-footer) · [c-breadcrumb](#c-breadcrumb) · [c-pagination](#c-pagination) · [c-dropdown](#c-dropdown)

**Conteúdo / seções:** [c-section-header](#c-section-header) · [c-hero](#c-hero) · [c-card](#c-card) · [c-cta](#c-cta) · [c-pricing](#c-pricing) · [c-testimonial](#c-testimonial) · [c-stats](#c-stats) · [c-logo-strip](#c-logo-strip) · [c-feature-list](#c-feature-list) · [c-steps](#c-steps-processo-numerado) · [c-accordion](#c-accordion-faq) · [c-tabs](#c-tabs) · [c-slider](#c-slider-carrossel) · [c-video](#c-video)

**Elementos / UI:** [c-button](#c-button) · [c-link](#c-link) · [c-badge](#c-badge) · [c-avatar](#c-avatar) · [c-divider](#c-divider) · [c-alert](#c-alert-banner--notificação) · [c-modal](#c-modal) · [c-tooltip](#c-tooltip) · [c-social](#c-social-links-de-redes-sociais) · [c-icon](#c-icon)

**Formulários:** ver [05-formularios](05-formularios.md) · **CMS/Blog:** ver [07-cms-richtext](07-cms-richtext.md)

- [Template para novos componentes](#template-para-novos-componentes)

---

## Como recriar no Webflow

Para cada componente:
1. Crie a estrutura HTML (Div Blocks / Link Blocks) aninhada como no exemplo.
2. Dê a **classe global** (`.c-button`) ao elemento raiz e estilize com o bloco "Block".
3. Dê as classes de **Element** (`.c-button__icon`) aos filhos.
4. Para variações: selecione o elemento e **adicione** a combo class (`.is--primary`).
5. Onde aparece `var(--algo)`, escolha a **Variable** correspondente no Webflow — não digite hex/px cru.

---

## c-button

📋 Status: especificado · Tag: `<a>` (link) ou `<button>` (ação)

### Estrutura

```html
<a class="c-button is--primary">
  <span class="c-button__label">Começar agora</span>
  <svg class="c-button__icon">...</svg>
</a>
```

### Block

```css
.c-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2xs);              /* 8px entre label e ícone */
  padding: 12px 24px;
  border: 1px solid transparent;
  border-radius: var(--radius-md);    /* 8px */
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 1rem;                    /* 16px */
  font-weight: 500;
  line-height: 1;
  text-align: center;
  text-decoration: none;
  white-space: nowrap;
  cursor: pointer;
  transition: background-color .2s ease, color .2s ease, border-color .2s ease, transform .1s ease;
}

.c-button:active {
  transform: translateY(1px);
}
```

### Elements

```css
.c-button__label {
  display: inline-block;
}

.c-button__icon {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  color: currentColor;   /* herda a cor do texto do botão (SVG com fill/stroke = currentColor) */
}
```

### Modifiers

```css
/* PRIMÁRIO — cor de marca */
.c-button.is--primary {
  background-color: var(--semantic-brand);   /* purple/5 #460095 */
  color: var(--semantic-text-inverse);       /* branco */
}
.c-button.is--primary:hover {
  background-color: var(--semantic-brand-hover);  /* purple/4-5 #6d2dbc */
}

/* SECUNDÁRIO — outline */
.c-button.is--secondary {
  background-color: transparent;
  color: var(--semantic-brand);
  border-color: var(--semantic-brand);
}
.c-button.is--secondary:hover {
  background-color: var(--color-purple-1);   /* #eee2ff */
}

/* GHOST — só texto */
.c-button.is--ghost {
  background-color: transparent;
  color: var(--semantic-text);
  border-color: transparent;
}
.c-button.is--ghost:hover {
  background-color: var(--semantic-surface-alt);  /* neutral/1 */
}

/* TAMANHOS */
.c-button.is--large {
  padding: 16px 32px;
  font-size: 1.125rem;   /* 18px */
}
.c-button.is--small {
  padding: 8px 16px;
  font-size: 0.875rem;   /* 14px */
}

/* LARGURA TOTAL (mobile / forms) */
.c-button.is--full {
  width: 100%;
}

/* BRANCO — para usar sobre seções escuras (is--dark) */
.c-button.is--white {
  background-color: var(--semantic-surface);
  color: var(--semantic-brand);
}
.c-button.is--white:hover { background-color: var(--color-neutral-1); }

/* SÓ ÍCONE (quadrado, sem label) */
.c-button.is--icon-only {
  padding: 12px;
  width: 44px;
  height: 44px;
}

/* CARREGANDO (esconde o label, mostra spinner) */
.c-button.is--loading {
  pointer-events: none;
  color: transparent;   /* esconde texto */
  position: relative;
}
.c-button.is--loading::after {
  content: "";
  position: absolute;
  width: 18px; height: 18px;
  border: 2px solid currentColor;
  border-top-color: transparent;
  border-radius: var(--radius-full);
  animation: c-spin 0.7s linear infinite;
}
@keyframes c-spin { to { transform: rotate(360deg); } }

/* DESABILITADO */
.c-button.is--disabled,
.c-button:disabled {
  opacity: 0.5;
  pointer-events: none;
  cursor: not-allowed;
}
```

**Notas:** SVG do ícone deve usar `fill="currentColor"` ou `stroke="currentColor"` para herdar a cor. Botão de ação real = `<button>`; navegação = `<a>`. Estados `is--white`/`is--icon-only`/`is--loading` cobrem os casos do dia a dia (ver catálogo em [08-combo-classes](08-combo-classes.md)).

---

## c-nav

📋 Status: especificado · Tag: `<header>` ou `<div>` com role de navegação

### Estrutura

```html
<header class="c-nav">
  <a class="c-nav__logo" href="/">...</a>
  <nav class="c-nav__menu">
    <a class="c-nav__link" href="#">Produto</a>
    <a class="c-nav__link is--active" href="#">Preços</a>
  </nav>
  <div class="c-nav__actions">
    <a class="c-button is--ghost">Entrar</a>
    <a class="c-button is--primary">Começar</a>
  </div>
  <button class="c-nav__toggle">...</button>
</header>
```

### Block

```css
.c-nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-lg);
  width: 100%;
  padding: var(--space-sm) 0;   /* horizontal vem do l-container que o envolve */
  background-color: var(--semantic-surface);
}
```

### Elements

```css
.c-nav__logo {
  display: inline-flex;
  align-items: center;
  flex-shrink: 0;
}
.c-nav__logo img,
.c-nav__logo svg {
  height: 32px;
  width: auto;
}

.c-nav__menu {
  display: flex;
  align-items: center;
  gap: var(--space-md);   /* 24px entre links */
}

.c-nav__link {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 1rem;
  font-weight: 500;
  line-height: 1;
  color: var(--semantic-text);
  text-decoration: none;
  padding: var(--space-2xs) 0;
  transition: color .2s ease;
}
.c-nav__link:hover {
  color: var(--semantic-brand);
}
.c-nav__link.is--active {
  color: var(--semantic-brand);
}

.c-nav__actions {
  display: flex;
  align-items: center;
  gap: var(--space-2xs);
  flex-shrink: 0;
}

/* Hambúrguer — escondido no desktop */
.c-nav__toggle {
  display: none;
  width: 44px;
  height: 44px;
  align-items: center;
  justify-content: center;
  background: transparent;
  border: 0;
  cursor: pointer;
}
```

### Modifiers

```css
/* Transparente (sobre hero) */
.c-nav.is--transparent {
  background-color: transparent;
}

/* Tema escuro */
.c-nav.is--dark {
  background-color: var(--color-purple-5);
}
.c-nav.is--dark .c-nav__link {
  color: var(--semantic-text-inverse);
}

/* Fixa no topo */
.c-nav.is--sticky {
  position: sticky;
  top: 0;
  z-index: 100;
}
```

### Responsivo (≤991px)

```css
/* @ Tablet/Mobile */
.c-nav__menu,
.c-nav__actions { display: none; }   /* vão para o menu mobile */
.c-nav__toggle { display: inline-flex; }
```

**Notas:** o comportamento sticky/transparent muda costuma ser controlado por **interação Webflow** vinculada à `.c-nav` (por isso não duplicar em global separada). O menu mobile abre via interação no `.c-nav__toggle`.

### Mega-menu (dropdown do "Product")

O turn.io tem um menu "Product" com vários itens (Helpdesk, Journeys, Playbooks, Reminders, Insights, Calling). Use `c-dropdown` + um painel em grid:

```html
<div class="c-nav__item c-dropdown">
  <button class="c-nav__link">Product</button>
  <div class="c-dropdown__list is--mega">
    <a class="c-mega__link" href="#">
      <svg class="c-icon is--brand">...</svg>
      <span class="c-mega__title">Helpdesk</span>
      <span class="c-mega__desc u-text-sm is--muted">Atendimento em escala</span>
    </a>
    <!-- repete para Journeys, Playbooks... -->
  </div>
</div>
```

```css
/* Painel largo em grade (sobre o c-dropdown__list base) */
.c-dropdown__list.is--mega {
  display: grid;
  grid-template-columns: repeat(2, minmax(220px, 1fr));
  gap: var(--space-2xs);
  min-width: 520px;
  padding: var(--space-sm);
}
.c-mega__link {
  display: grid;
  grid-template-columns: auto 1fr;
  grid-template-rows: auto auto;
  column-gap: var(--space-2xs);
  padding: var(--space-2xs);
  border-radius: var(--radius-md);
  text-decoration: none;
  transition: background-color var(--transition-base) var(--ease-standard);
}
.c-mega__link:hover { background-color: var(--semantic-surface-alt); }
.c-mega__link .c-icon { grid-row: span 2; align-self: center; }
.c-mega__title { font-weight: 500; color: var(--semantic-text); }
```

---

## c-hero

📋 Status: especificado

### Estrutura

```html
<div class="c-hero">
  <div class="c-hero__content">
    <span class="c-hero__eyebrow u-text-caption">Novidade</span>
    <h1 class="c-hero__title u-text-display">Conversas em escala</h1>
    <p class="c-hero__subtitle u-text-body-lg is--muted">Plataforma de WhatsApp...</p>
    <div class="c-hero__actions">
      <a class="c-button is--primary">Começar</a>
      <a class="c-button is--ghost">Saiba mais</a>
    </div>
  </div>
  <div class="c-hero__media">
    <img class="c-hero__image" src="..." alt="...">
  </div>
</div>
```

### Block

```css
.c-hero {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  gap: var(--space-3xl);   /* 96px entre conteúdo e mídia */
  width: 100%;
}
```

### Elements

```css
.c-hero__content {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: var(--space-md);   /* 24px entre eyebrow/título/sub/ações */
}

.c-hero__eyebrow {
  color: var(--semantic-brand);
}

/* título e subtítulo herdam o visual das classes u-text-* aplicadas em conjunto;
   aqui só ajustes específicos do hero, se houver */
.c-hero__title { max-width: 12ch; }
.c-hero__subtitle { max-width: 50ch; }

.c-hero__actions {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-2xs);
  margin-top: var(--space-2xs);
}

.c-hero__media {
  display: flex;
  justify-content: center;
}

.c-hero__image {
  width: 100%;
  height: auto;
  border-radius: var(--radius-lg);   /* 16px */
}
```

### Modifiers

```css
/* Centralizado, sem mídia lateral (texto centralizado em 1 coluna) */
.c-hero.is--centered {
  grid-template-columns: 1fr;
  justify-items: center;
  text-align: center;
}
.c-hero.is--centered .c-hero__content {
  align-items: center;
  max-width: 720px;
}

/* Fundo escuro (texto claro) */
.c-hero.is--dark { color: var(--semantic-text-inverse); }

/* Compacto (páginas internas) */
.c-hero.is--compact { gap: var(--space-xl); }
```

### Responsivo (≤767px)

```css
.c-hero {
  grid-template-columns: 1fr;   /* empilha conteúdo e mídia */
  gap: var(--space-xl);
}
.c-hero__actions .c-button { width: 100%; }   /* ou aplicar is--full no Designer */
```

**Notas:** a `.c-hero__image` costuma ser o **LCP** da página — carregar `eager` + `fetchpriority="high"`, não lazy (ver [10-performance](10-performance.md)). Padding vertical fica na `c-section`, não aqui.

---

## c-card

📋 Status: especificado

### Estrutura

```html
<div class="c-card">
  <div class="c-card__media"><img src="..." alt=""></div>
  <div class="c-card__body">
    <h3 class="c-card__title u-text-h4">Título</h3>
    <p class="c-card__text u-text-body is--muted">Descrição curta.</p>
    <a class="c-card__footer u-text-sm">Saiba mais →</a>
  </div>
</div>
```

### Block

```css
.c-card {
  display: flex;
  flex-direction: column;
  height: 100%;                                /* iguala altura em grid */
  background-color: var(--semantic-surface);
  border: 1px solid var(--semantic-border);    /* neutral/2 */
  border-radius: var(--radius-lg);             /* 16px */
  overflow: hidden;
  transition: box-shadow .2s ease, transform .2s ease;
}
.c-card:hover {
  box-shadow: var(--shadow-md);
  transform: translateY(-2px);
}
```

### Elements

```css
.c-card__media {
  width: 100%;
  aspect-ratio: 16 / 9;
  overflow: hidden;
}
.c-card__media img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.c-card__body {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: var(--space-2xs);    /* 8px */
  padding: var(--space-md); /* 24px */
  flex: 1;                  /* ocupa o resto da altura, empurra footer pra baixo */
}

.c-card__title { color: var(--semantic-text); }
.c-card__text  { color: var(--semantic-text-muted); }

.c-card__footer {
  margin-top: auto;         /* cola no rodapé do card */
  color: var(--semantic-brand);
  font-weight: 500;
  text-decoration: none;
}
```

### Modifiers

```css
/* Destaque */
.c-card.is--featured {
  border-color: var(--semantic-brand);
  box-shadow: var(--shadow-md);
}

/* Compacto */
.c-card.is--compact .c-card__body { padding: var(--space-sm); }

/* Horizontal (mídia ao lado) */
.c-card.is--horizontal {
  flex-direction: row;
}
.c-card.is--horizontal .c-card__media {
  width: 40%;
  aspect-ratio: auto;
}
```

**Notas:** para card inteiro clicável, use Link Block como raiz `.c-card`. Em grid, `height:100%` + `flex:1` no body alinha cards de alturas diferentes.

---

## c-cta

📋 Status: especificado

### Estrutura

```html
<div class="c-cta">
  <h2 class="c-cta__title u-text-h2">Pronto para começar?</h2>
  <p class="c-cta__text u-text-body-lg">Crie sua conta em minutos.</p>
  <div class="c-cta__actions">
    <a class="c-button is--primary is--large">Começar grátis</a>
  </div>
</div>
```

### Block + Elements

```css
.c-cta {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: var(--space-md);
  width: 100%;
}

.c-cta__title { color: inherit; max-width: 20ch; }
.c-cta__text  { color: inherit; opacity: .9; max-width: 50ch; }

.c-cta__actions {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: var(--space-2xs);
  margin-top: var(--space-2xs);
}
```

### Modifiers

```css
/* Alinhado à esquerda (em vez de centralizado) */
.c-cta.is--left {
  align-items: flex-start;
  text-align: left;
}
.c-cta.is--left .c-cta__actions { justify-content: flex-start; }
```

> Geralmente fica dentro de `.c-section.is--dark` — o `color: inherit` faz o texto herdar o branco da section.

---

## c-footer

📋 Status: especificado · Tag: `<footer>`

### Estrutura

```html
<footer class="c-footer">
  <div class="c-footer__top">
    <div class="c-footer__brand">...</div>
    <div class="c-footer__col">
      <span class="c-footer__heading u-text-caption">Produto</span>
      <a class="c-footer__link" href="#">Recursos</a>
      <a class="c-footer__link" href="#">Preços</a>
    </div>
  </div>
  <div class="c-footer__bottom">
    <span class="u-text-sm is--muted">© 2026 turn.io</span>
  </div>
</footer>
```

### Block + Elements

```css
.c-footer {
  display: flex;
  flex-direction: column;
  gap: var(--space-xl);
  width: 100%;
}

.c-footer__top {
  display: grid;
  grid-template-columns: 1.5fr repeat(3, 1fr);
  gap: var(--space-lg);
}

.c-footer__brand {
  display: flex;
  flex-direction: column;
  gap: var(--space-sm);
  max-width: 280px;
}

.c-footer__col {
  display: flex;
  flex-direction: column;
  gap: var(--space-2xs);
}

.c-footer__heading {
  color: var(--semantic-text-muted);
  margin-bottom: var(--space-2xs);
}

.c-footer__link {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 0.9375rem;   /* 15px */
  color: var(--semantic-text);
  text-decoration: none;
  transition: color .2s ease;
}
.c-footer__link:hover { color: var(--semantic-brand); }

.c-footer__bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: var(--space-sm);
  padding-top: var(--space-lg);
  border-top: 1px solid var(--semantic-border);
}
```

### Responsivo (≤767px)

```css
.c-footer__top { grid-template-columns: 1fr 1fr; }   /* 2 colunas */
```

---

## c-link

📋 Status: especificado · Tag: `<a>`

Link de texto inline (dentro de parágrafos, footer já tem o seu).

```css
.c-link {
  color: var(--semantic-brand);
  text-decoration: underline;
  text-underline-offset: 2px;
  transition: color var(--transition-base) var(--ease-standard);
}
.c-link:hover { color: var(--semantic-brand-hover); }

/* Link com seta (saiba mais →) */
.c-link.is--arrow {
  display: inline-flex;
  align-items: center;
  gap: var(--space-3xs);
  text-decoration: none;
  font-weight: 500;
}
.c-link.is--inverse { color: var(--semantic-text-inverse); }
```

---

## c-badge

📋 Status: especificado · Tag: `<span>`

Etiqueta pequena (status, categoria, "Novo").

```css
.c-badge {
  display: inline-flex;
  align-items: center;
  gap: var(--space-3xs);
  padding: 4px 10px;
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 0.75rem;       /* 12px */
  font-weight: 500;
  line-height: 1;
  letter-spacing: 0.02em;
  border-radius: var(--radius-full);
  background-color: var(--color-purple-1);   /* default: roxo claro */
  color: var(--color-purple-5);
  white-space: nowrap;
}

/* Variações de cor semântica */
.c-badge.is--success { background-color: var(--color-green-1); color: var(--color-green-4-5); }
.c-badge.is--error   { background-color: var(--color-red-1);   color: var(--color-red-4-5); }
.c-badge.is--warning { background-color: var(--color-yellow-1);color: var(--color-yellow-4-5); }
.c-badge.is--neutral { background-color: var(--semantic-surface-alt); color: var(--semantic-text); }

/* Com ponto de status */
.c-badge__dot {
  width: 6px; height: 6px;
  border-radius: var(--radius-full);
  background-color: currentColor;
}
```

---

## c-accordion (FAQ)

📋 Status: especificado · Tag: lista de itens

Perguntas que expandem. No Webflow, o abrir/fechar é uma **interação** (ou usar o componente Dropdown adaptado).

### Estrutura

```html
<div class="c-accordion">
  <div class="c-accordion__item">
    <button class="c-accordion__header">
      <span class="c-accordion__question u-text-h4">Pergunta?</span>
      <svg class="c-accordion__icon">...</svg>
    </button>
    <div class="c-accordion__panel">
      <p class="c-accordion__answer u-text-body is--muted">Resposta.</p>
    </div>
  </div>
</div>
```

### CSS

```css
.c-accordion {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.c-accordion__item {
  border-bottom: 1px solid var(--semantic-border);
}

.c-accordion__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-sm);
  width: 100%;
  padding: var(--space-md) 0;
  background: transparent;
  border: 0;
  text-align: left;
  cursor: pointer;
}

.c-accordion__icon {
  width: 24px; height: 24px;
  flex-shrink: 0;
  color: var(--semantic-brand);
  transition: transform var(--transition-slow) var(--ease-standard);
}
/* Quando aberto (combo aplicada via interação) */
.c-accordion__item.is--open .c-accordion__icon { transform: rotate(180deg); }

.c-accordion__panel {
  overflow: hidden;
  /* altura controlada por interação Webflow (height auto/0) */
}

.c-accordion__answer {
  padding-bottom: var(--space-md);
  max-width: 70ch;
}
```

---

## c-tabs

📋 Status: especificado

Usa o componente nativo **Tabs** do Webflow; estilize as classes:

```css
.c-tabs__menu {
  display: flex;
  gap: var(--space-2xs);
  border-bottom: 1px solid var(--semantic-border);
}

.c-tabs__link {
  padding: var(--space-2xs) var(--space-sm);
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 1rem;
  font-weight: 500;
  color: var(--semantic-text-muted);
  border-bottom: 2px solid transparent;
  cursor: pointer;
  transition: color var(--transition-base) var(--ease-standard),
              border-color var(--transition-base) var(--ease-standard);
}
.c-tabs__link:hover { color: var(--semantic-text); }

/* Aba ativa — no Webflow a classe nativa é .w--current */
.c-tabs__link.w--current {
  color: var(--semantic-brand);
  border-bottom-color: var(--semantic-brand);
}

.c-tabs__content {
  padding-top: var(--space-lg);
}
```

---

## c-logo-strip

📋 Status: especificado

Faixa de logos de clientes ("usado por").

```css
.c-logo-strip {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  gap: var(--space-xl);   /* 48px entre logos */
}

.c-logo-strip__item {
  height: 32px;
  width: auto;
  opacity: 0.6;
  filter: grayscale(100%);
  transition: opacity var(--transition-base) var(--ease-standard),
              filter var(--transition-base) var(--ease-standard);
}
.c-logo-strip__item:hover {
  opacity: 1;
  filter: grayscale(0%);
}
```

---

## c-pricing

📋 Status: especificado

Card de plano de preço. Reaproveita estrutura de card mas com identidade própria.

### Estrutura

```html
<div class="c-pricing is--featured">
  <span class="c-pricing__badge c-badge">Mais popular</span>
  <h3 class="c-pricing__name u-text-h3">Pro</h3>
  <div class="c-pricing__price">
    <span class="c-pricing__amount u-text-display">R$99</span>
    <span class="c-pricing__period u-text-sm is--muted">/mês</span>
  </div>
  <ul class="c-pricing__features">
    <li class="c-pricing__feature">Recurso A</li>
  </ul>
  <a class="c-button is--primary is--full">Assinar</a>
</div>
```

### CSS

```css
.c-pricing {
  display: flex;
  flex-direction: column;
  gap: var(--space-md);
  height: 100%;
  padding: var(--space-xl);
  background-color: var(--semantic-surface);
  border: 1px solid var(--semantic-border);
  border-radius: var(--radius-lg);
}

.c-pricing__price {
  display: flex;
  align-items: baseline;
  gap: var(--space-3xs);
}

.c-pricing__features {
  display: flex;
  flex-direction: column;
  gap: var(--space-2xs);
  list-style: none;
  margin: 0;
  padding: 0;
  flex: 1;
}

.c-pricing__feature {
  display: flex;
  align-items: flex-start;
  gap: var(--space-2xs);
  font-size: 0.9375rem;
  color: var(--semantic-text);
}
.c-pricing__feature::before {
  content: "";
  width: 18px; height: 18px;
  flex-shrink: 0;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='18' height='18' viewBox='0 0 18 18'%3E%3Cpath fill='none' stroke='%231dbf73' stroke-width='2' d='M3 9l4 4 8-9'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
}

.c-pricing__badge {
  align-self: flex-start;
}

/* Plano destacado */
.c-pricing.is--featured {
  border-color: var(--semantic-brand);
  border-width: 2px;
  box-shadow: var(--shadow-lg);
}
```

---

## c-testimonial

📋 Status: especificado

Depoimento de cliente.

```css
.c-testimonial {
  display: flex;
  flex-direction: column;
  gap: var(--space-md);
  padding: var(--space-xl);
  background-color: var(--semantic-surface-alt);
  border-radius: var(--radius-lg);
}

.c-testimonial__quote {
  font-size: 1.25rem;       /* 20px */
  line-height: 1.5;
  color: var(--semantic-text);
}

.c-testimonial__author {
  display: flex;
  align-items: center;
  gap: var(--space-2xs);
}

.c-testimonial__avatar {
  width: 48px; height: 48px;
  border-radius: var(--radius-full);
  object-fit: cover;
  flex-shrink: 0;
}

.c-testimonial__name { font-weight: 500; color: var(--semantic-text); }
.c-testimonial__role { font-size: 0.875rem; color: var(--semantic-text-muted); }
```

---

## c-stats

📋 Status: especificado

Bloco de métricas/números de impacto.

```css
.c-stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--space-lg);
  width: 100%;
}

.c-stat {
  display: flex;
  flex-direction: column;
  gap: var(--space-3xs);
  text-align: center;
}

.c-stat__number {
  font-size: clamp(2.5rem, 1.5rem + 4vw, 3.5rem);   /* 40 → 56px */
  font-weight: 700;
  line-height: 1;
  color: var(--semantic-brand);
}

.c-stat__label {
  font-size: 1rem;
  color: var(--semantic-text-muted);
}

/* Responsivo */
/* @ Mobile Landscape (≤767px) */
.c-stats { grid-template-columns: 1fr; }
```

---

## c-alert (banner / notificação)

📋 Status: especificado

Faixa de aviso (cookie, anúncio, erro de página).

```css
.c-alert {
  display: flex;
  align-items: center;
  gap: var(--space-2xs);
  padding: var(--space-sm) var(--space-md);
  border-radius: var(--radius-md);
  font-size: 0.9375rem;
  background-color: var(--color-blue-1);
  color: var(--color-blue-5);
}
.c-alert__icon { width: 20px; height: 20px; flex-shrink: 0; }
.c-alert__close {
  margin-left: auto;
  background: transparent; border: 0; cursor: pointer;
  color: inherit;
}

.c-alert.is--success { background-color: var(--color-green-1); color: var(--color-green-5); }
.c-alert.is--error   { background-color: var(--color-red-1);   color: var(--color-red-5); }
.c-alert.is--warning { background-color: var(--color-yellow-1);color: var(--color-yellow-5); }
```

---

## c-modal

📋 Status: especificado

Diálogo sobreposto. Abrir/fechar via interação Webflow.

### Estrutura

```html
<div class="c-modal">
  <div class="c-modal__overlay"></div>
  <div class="c-modal__dialog">
    <button class="c-modal__close">✕</button>
    <div class="c-modal__content">...</div>
  </div>
</div>
```

### CSS

```css
.c-modal {
  position: fixed;
  inset: 0;
  z-index: var(--z-modal);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-md);
  /* display controlado por interação (none → flex) */
}

.c-modal__overlay {
  position: absolute;
  inset: 0;
  z-index: var(--z-overlay);
  background-color: rgba(0, 0, 0, 0.5);
}

.c-modal__dialog {
  position: relative;
  z-index: var(--z-modal);
  width: 100%;
  max-width: 480px;
  max-height: 90vh;
  overflow-y: auto;
  padding: var(--space-xl);
  background-color: var(--semantic-surface);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
}

.c-modal__close {
  position: absolute;
  top: var(--space-sm);
  right: var(--space-sm);
  width: 40px; height: 40px;
  display: flex; align-items: center; justify-content: center;
  background: transparent; border: 0; cursor: pointer;
  color: var(--semantic-text-muted);
  border-radius: var(--radius-full);
}
.c-modal__close:hover { background-color: var(--semantic-surface-alt); }
```

**Notas:** ao abrir, trave o scroll do body (`overflow: hidden` no body via interação). Fechar com clique no overlay e tecla Esc (custom code para acessibilidade).

---

## c-dropdown

📋 Status: especificado

Menu suspenso (usado na nav, em ações). Webflow tem componente Dropdown nativo.

```css
.c-dropdown {
  position: relative;
  display: inline-block;
}

.c-dropdown__list {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: var(--z-dropdown);
  min-width: 200px;
  margin-top: var(--space-3xs);
  padding: var(--space-2xs);
  background-color: var(--semantic-surface);
  border: 1px solid var(--semantic-border);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-lg);
  /* visibilidade controlada por interação */
}

.c-dropdown__link {
  display: block;
  padding: var(--space-2xs) var(--space-sm);
  font-size: 0.9375rem;
  color: var(--semantic-text);
  text-decoration: none;
  border-radius: var(--radius-sm);
  transition: background-color var(--transition-base) var(--ease-standard);
}
.c-dropdown__link:hover { background-color: var(--semantic-surface-alt); }
```

---

## c-avatar

📋 Status: especificado

```css
.c-avatar {
  width: 40px; height: 40px;
  border-radius: var(--radius-full);
  object-fit: cover;
  flex-shrink: 0;
  background-color: var(--semantic-surface-alt);
}
.c-avatar.is--sm { width: 32px; height: 32px; }
.c-avatar.is--lg { width: 64px; height: 64px; }

/* Grupo sobreposto (ex.: "+5 pessoas") */
.c-avatar-group { display: inline-flex; }
.c-avatar-group .c-avatar {
  border: 2px solid var(--semantic-surface);
  margin-left: -8px;
}
.c-avatar-group .c-avatar:first-child { margin-left: 0; }
```

---

## c-divider

📋 Status: especificado

```css
.c-divider {
  width: 100%;
  height: 1px;
  border: 0;
  background-color: var(--semantic-border);
}
.c-divider.is--vertical {
  width: 1px;
  height: auto;
  align-self: stretch;
}
```

---

## c-feature-list

📋 Status: especificado

Lista de itens com ícone de check (recursos, benefícios).

```css
.c-feature-list {
  display: flex;
  flex-direction: column;
  gap: var(--space-sm);
  list-style: none;
  margin: 0;
  padding: 0;
}

.c-feature-list__item {
  display: flex;
  align-items: flex-start;
  gap: var(--space-2xs);
  font-size: 1rem;
  line-height: 1.5;
  color: var(--semantic-text);
}

.c-feature-list__icon {
  width: 24px; height: 24px;
  flex-shrink: 0;
  color: var(--semantic-success);
}
```

---

## c-breadcrumb

📋 Status: especificado · Tag: `<nav>`

```css
.c-breadcrumb {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: var(--space-3xs);
  font-size: 0.875rem;
}
.c-breadcrumb__link {
  color: var(--semantic-text-muted);
  text-decoration: none;
}
.c-breadcrumb__link:hover { color: var(--semantic-brand); }
.c-breadcrumb__sep { color: var(--semantic-text-muted); }
.c-breadcrumb__current { color: var(--semantic-text); font-weight: 500; }
```

---

## c-pagination

📋 Status: especificado

```css
.c-pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-3xs);
}
.c-pagination__item {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 40px;
  height: 40px;
  padding: 0 var(--space-2xs);
  font-size: 0.9375rem;
  color: var(--semantic-text);
  text-decoration: none;
  border-radius: var(--radius-md);
  transition: background-color var(--transition-base) var(--ease-standard);
}
.c-pagination__item:hover { background-color: var(--semantic-surface-alt); }
.c-pagination__item.is--active {
  background-color: var(--semantic-brand);
  color: var(--semantic-text-inverse);
}
.c-pagination__item.is--disabled { opacity: 0.4; pointer-events: none; }
```

---

## c-section-header

📋 Status: especificado

Padrão repetido em quase toda seção: eyebrow + título + subtítulo centralizados. Vale ter como componente para não recriar à mão.

### Estrutura

```html
<div class="c-section-header is--center">
  <span class="c-section-header__eyebrow u-text-caption">Recursos</span>
  <h2 class="c-section-header__title u-text-h2">Tudo que você precisa</h2>
  <p class="c-section-header__subtitle u-text-body-lg is--muted">Subtítulo da seção.</p>
</div>
```

```css
.c-section-header {
  display: flex;
  flex-direction: column;
  gap: var(--space-2xs);
  margin-bottom: var(--space-2xl);   /* espaço até o conteúdo da seção */
}
.c-section-header__eyebrow { color: var(--semantic-brand); }
.c-section-header__title { max-width: 24ch; }
.c-section-header__subtitle { max-width: 60ch; }

/* Centralizado */
.c-section-header.is--center {
  align-items: center;
  text-align: center;
}
.c-section-header.is--center .c-section-header__title,
.c-section-header.is--center .c-section-header__subtitle {
  margin-left: auto;
  margin-right: auto;
}
```

---

## c-slider (carrossel)

📋 Status: especificado · usa o **Slider** nativo do Webflow

O turn.io usa slider (depoimentos/cases). Estilize as classes nativas (`w-slider`, `w-slide`, setas, dots) via classes suas.

```css
.c-slider {
  position: relative;
  width: 100%;
}

/* Máscara (área visível) — w-slider-mask */
.c-slider .w-slider-mask {
  overflow: hidden;
}

/* Cada slide — w-slide */
.c-slider__slide {
  padding: var(--space-2xs);   /* respiro entre slides */
}

/* Setas */
.c-slider__arrow {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48px; height: 48px;
  background-color: var(--semantic-surface);
  border: 1px solid var(--semantic-border);
  border-radius: var(--radius-full);
  box-shadow: var(--shadow-sm);
  color: var(--semantic-text);
}
.c-slider__arrow:hover { background-color: var(--semantic-surface-alt); }

/* Dots de navegação — w-slider-nav */
.c-slider .w-slider-nav {
  padding-top: var(--space-sm);
}
.c-slider .w-slider-dot {
  width: 8px; height: 8px;
  background-color: var(--semantic-border);
}
.c-slider .w-slider-dot.w-active {
  background-color: var(--semantic-brand);
}
```

> No Webflow, adicione classes suas (`c-slider__arrow`, etc.) sobre os elementos do Slider em vez de estilizar a classe `.w-*` pura.

---

## c-video

📋 Status: especificado

Wrapper de vídeo/embed responsivo (mantém 16:9 sem CLS).

```css
.c-video {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: var(--radius-lg);
  overflow: hidden;
  background-color: var(--color-neutral-2);
}
.c-video iframe,
.c-video video,
.c-video .w-embed {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  border: 0;
}

/* Outras proporções */
.c-video.is--square   { aspect-ratio: 1 / 1; }
.c-video.is--portrait { aspect-ratio: 9 / 16; }
```

> Use **lite-embed**/clique-para-tocar em vez de autoplay de YouTube pesado (ver [10-performance](10-performance.md)).

---

## c-tooltip

📋 Status: especificado

```css
.c-tooltip {
  position: relative;
  display: inline-flex;
}
.c-tooltip__bubble {
  position: absolute;
  bottom: calc(100% + 8px);
  left: 50%;
  transform: translateX(-50%);
  z-index: var(--z-dropdown);
  padding: var(--space-3xs) var(--space-2xs);
  font-size: 0.8125rem;
  white-space: nowrap;
  color: var(--semantic-text-inverse);
  background-color: var(--color-neutral-5);
  border-radius: var(--radius-sm);
  opacity: 0;
  pointer-events: none;
  transition: opacity var(--transition-base) var(--ease-standard);
}
.c-tooltip:hover .c-tooltip__bubble,
.c-tooltip:focus-within .c-tooltip__bubble {
  opacity: 1;
}
```

---

## c-steps (processo numerado)

📋 Status: especificado

"Como funciona" em passos.

```css
.c-steps {
  display: flex;
  flex-direction: column;
  gap: var(--space-lg);
  counter-reset: step;
}
.c-step {
  display: flex;
  align-items: flex-start;
  gap: var(--space-sm);
}
.c-step__number {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px; height: 40px;
  border-radius: var(--radius-full);
  background-color: var(--semantic-brand);
  color: var(--semantic-text-inverse);
  font-weight: 700;
}
.c-step__body { display: flex; flex-direction: column; gap: var(--space-3xs); }

/* Horizontal (3 passos lado a lado) */
.c-steps.is--horizontal {
  flex-direction: row;
}
/* @ Mobile (≤767px) volta a empilhar */
```

---

## c-social (links de redes sociais)

📋 Status: especificado

```css
.c-social {
  display: flex;
  align-items: center;
  gap: var(--space-2xs);
}
.c-social__link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px; height: 40px;
  border-radius: var(--radius-full);
  color: var(--semantic-text-muted);
  transition: color var(--transition-base) var(--ease-standard),
              background-color var(--transition-base) var(--ease-standard);
}
.c-social__link:hover {
  color: var(--semantic-brand);
  background-color: var(--semantic-surface-alt);
}
.c-social__link svg { width: 20px; height: 20px; }

.c-social.is--inverse .c-social__link { color: rgba(255,255,255,0.7); }
.c-social.is--inverse .c-social__link:hover { color: var(--semantic-text-inverse); }
```

---

## c-icon

📋 Status: especificado · convenção, não componente visual

Padrão de uso de ícones (SVG). Sempre **SVG inline** com `currentColor` para herdar cor e permitir combos.

```css
.c-icon {
  display: inline-block;
  width: 24px;
  height: 24px;
  flex-shrink: 0;
  color: currentColor;   /* herda a cor do contexto */
}
.c-icon.is--sm { width: 18px; height: 18px; }
.c-icon.is--lg { width: 32px; height: 32px; }
.c-icon.is--brand { color: var(--semantic-brand); }
```

O SVG deve usar `fill="currentColor"` ou `stroke="currentColor"` (sem cor fixa no arquivo). No Webflow, cole o SVG num **Embed** ou use o HTML Embed inline.

> **Nunca** ícone em PNG/JPG — SVG escala sem perda, herda cor e pesa menos (ver [10-performance](10-performance.md)).

---

## Template para novos componentes

```markdown
## c-[nome]

📋 Status: especificado · Tag: `<...>`

### Estrutura
```html
<div class="c-[nome]"> ... </div>
```

### Block
```css
.c-[nome] { /* propriedades completas */ }
```

### Elements
```css
.c-[nome]__[parte] { /* ... */ }
```

### Modifiers
```css
.c-[nome].is--[variação] { /* só o que difere da base */ }
```

**Notas:** [acessibilidade, performance, responsivo]
```

> **Antes de criar:** já existe em cima? Dá pra resolver com um **modifier** de um componente existente? (checklist em [11-fluxo-de-trabalho](11-fluxo-de-trabalho.md)).

---

**Anterior:** [← 03 — Espaçamento & Layout](03-espacamento-layout.md) | **Próximo:** [05 — Formulários →](05-formularios.md)
