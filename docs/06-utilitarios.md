# 06 — Utilitários

> Classes de **propósito único** (namespace `u-`) para ajustes pontuais que não justificam um componente. É a parte mais "Tailwind" do sistema — mas use com parcimônia: utilitário demais vira o caos que o BEM evita.

**Quando usar `u-`:** um ajuste isolado e repetível (esconder em mobile, centralizar texto, espaçar). **Quando NÃO usar:** se você está empilhando 5 utilitários para construir um componente — isso é um componente, dê um nome `c-`.

---

## Sumário

1. [Tipografia](#1-tipografia) (ver também [02](02-tipografia.md))
2. [Visibilidade / responsivo](#2-visibilidade--responsivo)
3. [Acessibilidade](#3-acessibilidade)
4. [Display & Flex](#4-display--flex)
5. [Espaçamento pontual](#5-espaçamento-pontual)
6. [Largura / altura](#6-largura--altura)
7. [Alinhamento de texto](#7-alinhamento-de-texto)
8. [Cor de fundo](#8-cor-de-fundo)
9. [Misc](#9-misc)

---

## 1. Tipografia

As classes `.u-text-*` (display, h1…caption) e seus modifiers estão documentadas em [02-tipografia](02-tipografia.md). São utilitários de texto reutilizáveis.

---

## 2. Visibilidade / responsivo

Esconder/mostrar por breakpoint. No Webflow você também pode usar o toggle de display por breakpoint no painel, mas estas classes deixam a intenção explícita e reutilizável.

```css
/* Esconde sempre */
.u-hidden { display: none !important; }

/* Esconde só no desktop (base) — útil p/ versão mobile de um bloco */
@media (min-width: 992px) {
  .u-hide-desktop { display: none !important; }
}

/* Esconde no tablet e abaixo */
@media (max-width: 991px) {
  .u-hide-tablet { display: none !important; }
}

/* Esconde no mobile landscape e abaixo */
@media (max-width: 767px) {
  .u-hide-mobile { display: none !important; }
}

/* Mostra só no mobile (escondido acima de 767px) */
@media (min-width: 768px) {
  .u-mobile-only { display: none !important; }
}
```

> `!important` aqui é aceitável: utilitários de visibilidade precisam vencer a especificidade do componente. É o único lugar onde `!important` é OK no projeto.

---

## 3. Acessibilidade

```css
/* Esconde visualmente mas mantém para leitores de tela (labels, headings de seção) */
.u-sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* Pula para o conteúdo (skip link) — visível só ao focar via teclado */
.u-skip-link {
  position: absolute;
  left: -9999px;
  z-index: var(--z-toast);
  padding: var(--space-2xs) var(--space-sm);
  background: var(--semantic-brand);
  color: var(--semantic-text-inverse);
  border-radius: var(--radius-md);
}
.u-skip-link:focus {
  left: var(--space-sm);
  top: var(--space-sm);
}
```

---

## 4. Display & Flex

Para micro-layouts pontuais (ícone + texto, alinhar 2 coisas). Para layout de página use `l-*` ([03](03-espacamento-layout.md)).

```css
.u-flex          { display: flex; }
.u-flex-col      { display: flex; flex-direction: column; }
.u-inline-flex   { display: inline-flex; }
.u-block         { display: block; }
.u-inline-block  { display: inline-block; }

.u-items-center  { align-items: center; }
.u-items-start   { align-items: flex-start; }
.u-items-end     { align-items: flex-end; }

.u-justify-center  { justify-content: center; }
.u-justify-between { justify-content: space-between; }
.u-justify-end     { justify-content: flex-end; }

.u-flex-wrap   { flex-wrap: wrap; }
.u-flex-1      { flex: 1; }
.u-flex-shrink-0 { flex-shrink: 0; }

/* gaps prontos */
.u-gap-xs  { gap: var(--space-2xs); }   /* 8px  */
.u-gap-sm  { gap: var(--space-sm); }    /* 16px */
.u-gap-md  { gap: var(--space-md); }    /* 24px */
.u-gap-lg  { gap: var(--space-lg); }    /* 32px */
```

---

## 5. Espaçamento pontual

Use só quando `gap`/`l-stack` não resolve. Prefira margem **em uma direção** (bottom) para evitar collapse.

```css
.u-mb-xs { margin-bottom: var(--space-2xs); }
.u-mb-sm { margin-bottom: var(--space-sm); }
.u-mb-md { margin-bottom: var(--space-md); }
.u-mb-lg { margin-bottom: var(--space-lg); }
.u-mb-xl { margin-bottom: var(--space-xl); }
.u-mb-0  { margin-bottom: 0; }

.u-mt-auto { margin-top: auto; }   /* empurra p/ baixo em flex */

.u-p-sm { padding: var(--space-sm); }
.u-p-md { padding: var(--space-md); }
.u-p-lg { padding: var(--space-lg); }
```

---

## 6. Largura / altura

```css
.u-w-full   { width: 100%; }
.u-h-full   { height: 100%; }
.u-max-w-prose { max-width: 65ch; }    /* leitura confortável */
.u-max-w-sm { max-width: 480px; }
.u-max-w-md { max-width: 720px; }
.u-max-w-lg { max-width: 960px; }
.u-mx-auto  { margin-left: auto; margin-right: auto; }
```

---

## 7. Alinhamento de texto

```css
.u-text-left   { text-align: left; }
.u-text-center { text-align: center; }
.u-text-right  { text-align: right; }
```

> Para alinhar texto dentro de um `.u-text-*` específico, prefira o modifier `.is--center` ([02](02-tipografia.md)). Estas são genéricas.

---

## 8. Cor de fundo

Atalhos para fundos pontuais (seções já usam `.c-section.is--*`).

```css
.u-bg-surface     { background-color: var(--semantic-surface); }     /* branco */
.u-bg-surface-alt { background-color: var(--semantic-surface-alt); } /* neutral/1 */
.u-bg-brand       { background-color: var(--semantic-brand); color: var(--semantic-text-inverse); }
.u-bg-purple-1    { background-color: var(--color-purple-1); }
.u-bg-green-1     { background-color: var(--color-green-1); }
```

---

## 9. Misc

```css
/* Imagem responsiva segura (nunca estoura o container) */
.u-img-responsive {
  display: block;
  max-width: 100%;
  height: auto;
}

/* Cantos arredondados pontuais */
.u-radius-md   { border-radius: var(--radius-md); }
.u-radius-lg   { border-radius: var(--radius-lg); }
.u-radius-full { border-radius: var(--radius-full); }

/* Sombra pontual */
.u-shadow-md { box-shadow: var(--shadow-md); }

/* Corta overflow */
.u-overflow-hidden { overflow: hidden; }

/* Truncar texto em 1 linha com reticências */
.u-truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* Limitar a N linhas (line clamp) — ex.: descrição de card */
.u-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.u-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

---

## Regra final sobre utilitários

> Se você precisa de **mais de 2-3 utilitários** num mesmo elemento para conseguir o visual, pare: provavelmente é um **componente** (`c-`) ou um padrão de layout (`l-`). Utilitário é tempero, não a receita.

---

**Anterior:** [← 05 — Formulários](05-formularios.md) | **Próximo:** [07 — CMS & Rich Text →](07-cms-richtext.md)
