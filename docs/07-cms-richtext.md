# 07 — CMS & Rich Text

> O turn.io tem **blog/recursos dinâmicos** (o site usa `w-dyn-list`, `w-dyn-item`, `w-richtext`). Esta é a parte que mais escapa da padronização porque o conteúdo é gerado pelo editor de CMS — então o estilo precisa estar **pré-definido** para sair certo automaticamente.

---

## Sumário

1. [Como o CMS do Webflow funciona (rápido)](#1-como-o-cms-do-webflow-funciona-rápido)
2. [c-richtext — estilizar o conteúdo do editor](#2-c-richtext--estilizar-o-conteúdo-do-editor)
3. [Collection List (lista dinâmica)](#3-collection-list-lista-dinâmica)
4. [c-post-card (card de post no CMS)](#4-c-post-card-card-de-post-no-cms)
5. [Estado vazio e paginação](#5-estado-vazio-e-paginação)
6. [Regras de ouro](#6-regras-de-ouro)

---

## 1. Como o CMS do Webflow funciona (rápido)

- Uma **Collection** (ex.: "Blog Posts") é como uma tabela. Cada item tem campos (título, slug, imagem, corpo Rich Text…).
- Uma **Collection List** (`w-dyn-list`) repete um template de item (`w-dyn-item`) para cada registro.
- Um campo **Rich Text** vira um elemento `w-richtext` cujo HTML interno (h2, p, ul, img, blockquote…) **você não controla item a item** — só via estilo da classe.

> Por isso o Rich Text precisa de estilo **descendente** (estilizar os filhos a partir de uma classe pai). É a única exceção saudável à regra "não aninhe seletores".

---

## 2. c-richtext — estilizar o conteúdo do editor

Aplique a classe `.c-richtext` no **Rich Text Element**. No Webflow, você estiliza os elementos internos selecionando "Nested selectors" dentro do Rich Text (All H2, All Paragraphs, etc.), o que gera CSS descendente como abaixo.

```css
.c-richtext {
  display: flex;
  flex-direction: column;
  gap: var(--space-md);
  max-width: 70ch;           /* leitura confortável */
  color: var(--semantic-text);
}

/* Títulos dentro do conteúdo */
.c-richtext h2 {
  font-size: clamp(1.75rem, 1.1rem + 2.5vw, 2.25rem);
  font-weight: 700;
  line-height: 1.15;
  margin-top: var(--space-lg);
}
.c-richtext h3 {
  font-size: clamp(1.375rem, 1rem + 1.5vw, 1.75rem);
  font-weight: 700;
  line-height: 1.2;
  margin-top: var(--space-md);
}
.c-richtext h4 {
  font-size: 1.375rem;
  font-weight: 500;
  margin-top: var(--space-md);
}

/* Parágrafos */
.c-richtext p {
  font-size: 1.0625rem;      /* 17px — leitura de artigo */
  line-height: 1.7;
}

/* Links no corpo */
.c-richtext a {
  color: var(--semantic-brand);
  text-decoration: underline;
  text-underline-offset: 2px;
}
.c-richtext a:hover { color: var(--semantic-brand-hover); }

/* Listas */
.c-richtext ul,
.c-richtext ol {
  display: flex;
  flex-direction: column;
  gap: var(--space-2xs);
  padding-left: var(--space-md);
}
.c-richtext li {
  font-size: 1.0625rem;
  line-height: 1.7;
}

/* Citação */
.c-richtext blockquote {
  padding: var(--space-sm) var(--space-md);
  border-left: 4px solid var(--semantic-brand);
  background-color: var(--semantic-surface-alt);
  border-radius: 0 var(--radius-md) var(--radius-md) 0;
  font-size: 1.25rem;
  font-style: italic;
  color: var(--semantic-text);
}

/* Imagens */
.c-richtext img {
  max-width: 100%;
  height: auto;
  border-radius: var(--radius-lg);
}

/* Figuras / legendas */
.c-richtext figure { margin: 0; }
.c-richtext figcaption {
  font-size: 0.875rem;
  color: var(--semantic-text-muted);
  text-align: center;
  margin-top: var(--space-2xs);
}

/* Código inline e bloco */
.c-richtext code {
  font-family: ui-monospace, SFMono-Regular, Menlo, monospace;
  font-size: 0.9em;
  padding: 2px 6px;
  background-color: var(--semantic-surface-alt);
  border-radius: var(--radius-sm);
}
.c-richtext pre {
  padding: var(--space-sm);
  background-color: var(--color-neutral-5);
  color: var(--semantic-text-inverse);
  border-radius: var(--radius-md);
  overflow-x: auto;
}

/* Divisor horizontal */
.c-richtext hr {
  border: 0;
  height: 1px;
  background-color: var(--semantic-border);
  margin: var(--space-md) 0;
}
```

> **Combo:** `.c-richtext.is--narrow` (max-width 60ch) e `.c-richtext.is--inverse` (texto claro p/ fundo escuro) cobrem variações comuns.

---

## 3. Collection List (lista dinâmica)

A Collection List do Webflow gera `w-dyn-list` (wrapper) → `w-dyn-items` (grid/flex) → `w-dyn-item` (cada item). Aplique suas classes de layout neles:

```html
<div class="c-collection w-dyn-list">
  <div class="l-grid is--auto w-dyn-items">
    <div class="w-dyn-item">
      <!-- template do item: c-post-card -->
      <a class="c-post-card"> ... </a>
    </div>
  </div>
</div>
```

```css
.c-collection { width: 100%; }

/* Aplique a grade no wrapper de itens (w-dyn-items) via uma classe sua */
.c-collection .w-dyn-items {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: var(--space-lg);
}
```

> Você pode (e deve) dar uma **classe global sua** ao `w-dyn-items` e ao `w-dyn-item` em vez de estilizar a classe nativa diretamente — assim mantém o padrão `l-grid`/`c-*`.

---

## 4. c-post-card (card de post no CMS)

Variante de `c-card` ([04](04-componentes.md)) ligada a campos do CMS. Estrutura com campos dinâmicos:

```html
<a class="c-post-card" href="#">
  <div class="c-post-card__media">
    <img class="c-post-card__image" src="[Imagem CMS]" alt="[Alt CMS]">
  </div>
  <div class="c-post-card__body">
    <span class="c-post-card__category c-badge">[Categoria]</span>
    <h3 class="c-post-card__title u-text-h4">[Título]</h3>
    <p class="c-post-card__excerpt u-text-body is--muted u-clamp-3">[Resumo]</p>
    <div class="c-post-card__meta">
      <span class="c-post-card__date u-text-sm is--muted">[Data]</span>
    </div>
  </div>
</a>
```

```css
.c-post-card {
  display: flex;
  flex-direction: column;
  height: 100%;
  background-color: var(--semantic-surface);
  border: 1px solid var(--semantic-border);
  border-radius: var(--radius-lg);
  overflow: hidden;
  text-decoration: none;
  transition: box-shadow var(--transition-base) var(--ease-standard),
              transform var(--transition-base) var(--ease-standard);
}
.c-post-card:hover {
  box-shadow: var(--shadow-md);
  transform: translateY(-2px);
}

.c-post-card__media { aspect-ratio: 16 / 9; overflow: hidden; }
.c-post-card__image {
  width: 100%; height: 100%;
  object-fit: cover;
  transition: transform var(--transition-slow) var(--ease-standard);
}
.c-post-card:hover .c-post-card__image { transform: scale(1.04); }

.c-post-card__body {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: var(--space-2xs);
  padding: var(--space-md);
  flex: 1;
}

.c-post-card__meta {
  margin-top: auto;
  padding-top: var(--space-2xs);
}
```

> A imagem do post deve ter **dimensão/aspect-ratio fixa** (16:9) para não causar CLS quando o CMS carrega imagens de tamanhos diferentes (ver [10-performance](10-performance.md)).

---

## 5. Estado vazio e paginação

```css
/* Estado vazio (nenhum item) — w-dyn-empty */
.c-collection .w-dyn-empty {
  padding: var(--space-xl);
  text-align: center;
  color: var(--semantic-text-muted);
  background-color: var(--semantic-surface-alt);
  border-radius: var(--radius-lg);
}
```

Para paginação use o componente **`c-pagination`** ([04](04-componentes.md)) estilizando os botões nativos do Webflow (`.w-pagination-next`, `.w-pagination-previous`).

---

## 6. Regras de ouro

1. **Estilize o Rich Text uma vez** na classe `.c-richtext`. Conteúdo do editor herda — ninguém formata post a post.
2. **Limite a largura do texto** (`max-width: 70ch`) para legibilidade.
3. **Imagens do CMS com aspect-ratio fixa** → zero CLS.
4. Dê **classes suas** aos wrappers nativos (`w-dyn-items`, `w-dyn-item`) em vez de estilizar a classe `.w-*` diretamente.
5. `u-clamp-2/3` ([06](06-utilitarios.md)) controla resumos de tamanho variável vindos do CMS.

---

**Anterior:** [← 06 — Utilitários](06-utilitarios.md) | **Próximo:** [08 — Catálogo de Combo Classes →](08-combo-classes.md)
