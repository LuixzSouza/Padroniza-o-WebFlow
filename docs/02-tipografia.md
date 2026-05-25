# 02 — Tipografia

> Como o texto é estruturado, nomeado e reutilizado. Fonte oficial: **DM Sans**.
> **Cada classe abaixo vem com o CSS completo pronto para recriar no Webflow.**

---

## Sumário

1. [Fonte](#1-fonte)
2. [Como ler / aplicar no Webflow](#2-como-ler--aplicar-no-webflow)
3. [Text Styles — CSS completo de cada classe](#3-text-styles--css-completo-de-cada-classe)
4. [Modifiers de texto](#4-modifiers-de-texto)
5. [Tags HTML vs. classes de estilo](#5-tags-html-vs-classes-de-estilo)
6. [Regras de uso](#6-regras-de-uso)

---

## 1. Fonte

**DM Sans** — já é a fonte do site atual, carregada via Google Fonts. Stack completa (define no token `font/family/base`):

```css
font-family: "DM Sans", Helvetica, Arial, sans-serif;
```

Pesos carregados: **400 / 500 / 700** (só esses três — ver [10-performance](10-performance.md)).

---

## 2. Como ler / aplicar no Webflow

Cada classe `.u-text-*` é uma **classe global** (não combo). Para criar no Designer:

1. Selecione um elemento de texto (Text Block, Heading, Paragraph).
2. No painel **Style**, dê o nome da classe (ex.: `u-text-h1`).
3. Aplique cada propriedade do bloco CSS abaixo no painel:
   - **Typography** → Font, Weight, Size, Line height, Letter spacing.
   - **Color** → escolha a Variable `semantic/text` (nunca hex cru).
4. Os valores de `font-size` usam `clamp()` (fluid type) — no Webflow, digite o `clamp(...)` no campo **Size** como valor custom, ou crie a Variable `font/size/*` com esse valor e use ela.

> `clamp(min, preferido, max)` = o tamanho nunca fica menor que `min` nem maior que `max`, e escala junto com a largura da tela no meio. Resolve responsividade sem override por breakpoint.

---

## 3. Text Styles — CSS completo de cada classe

```css
/* ---------- DISPLAY (hero principal) ---------- */
.u-text-display {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: clamp(2.5rem, 1rem + 7vw, 4rem);   /* 40px → 64px */
  font-weight: 700;
  line-height: 1.05;
  letter-spacing: -0.02em;
  color: var(--semantic-text);
  margin: 0;
}

/* ---------- H1 (título de página) ---------- */
.u-text-h1 {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: clamp(2rem, 1.2rem + 4vw, 3rem);    /* 32px → 48px */
  font-weight: 700;
  line-height: 1.1;
  letter-spacing: -0.02em;
  color: var(--semantic-text);
  margin: 0;
}

/* ---------- H2 (título de seção) ---------- */
.u-text-h2 {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: clamp(1.75rem, 1.1rem + 2.5vw, 2.25rem);  /* 28px → 36px */
  font-weight: 700;
  line-height: 1.15;
  letter-spacing: -0.01em;
  color: var(--semantic-text);
  margin: 0;
}

/* ---------- H3 (subtítulo) ---------- */
.u-text-h3 {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: clamp(1.375rem, 1rem + 1.5vw, 1.75rem);   /* 22px → 28px */
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: 0;
  color: var(--semantic-text);
  margin: 0;
}

/* ---------- H4 (card title) ---------- */
.u-text-h4 {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 1.375rem;   /* 22px */
  font-weight: 500;
  line-height: 1.3;
  color: var(--semantic-text);
  margin: 0;
}

/* ---------- BODY LARGE (lead / intro) ---------- */
.u-text-body-lg {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 1.125rem;   /* 18px */
  font-weight: 400;
  line-height: 1.5;
  color: var(--semantic-text);
  margin: 0;
}

/* ---------- BODY (texto padrão) ---------- */
.u-text-body {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 1rem;       /* 16px */
  font-weight: 400;
  line-height: 1.5;
  color: var(--semantic-text);
  margin: 0;
}

/* ---------- SMALL (legendas, labels) ---------- */
.u-text-sm {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 0.875rem;   /* 14px */
  font-weight: 400;
  line-height: 1.5;
  color: var(--semantic-text);
  margin: 0;
}

/* ---------- CAPTION / EYEBROW (metadados) ---------- */
.u-text-caption {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 0.75rem;    /* 12px */
  font-weight: 500;
  line-height: 1.4;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--semantic-text-muted);
  margin: 0;
}
```

> **Por que `margin: 0`:** o espaçamento entre textos é responsabilidade do layout (`gap` / `l-stack` — ver [03](03-espacamento-layout.md)), não da classe de texto. Margin embutida em heading é fonte clássica de espaçamento inconsistente.

### Tabela de referência rápida

| Classe | Size (min→max) | Weight | Line-height |
|---|---|---|---|
| `.u-text-display` | 40→64px | 700 | 1.05 |
| `.u-text-h1` | 32→48px | 700 | 1.1 |
| `.u-text-h2` | 28→36px | 700 | 1.15 |
| `.u-text-h3` | 22→28px | 700 | 1.2 |
| `.u-text-h4` | 22px | 500 | 1.3 |
| `.u-text-body-lg` | 18px | 400 | 1.5 |
| `.u-text-body` | 16px | 400 | 1.5 |
| `.u-text-sm` | 14px | 400 | 1.5 |
| `.u-text-caption` | 12px | 500 | 1.4 |

---

## 4. Modifiers de texto

Combo classes (`is--*`) adicionadas em cima de uma `.u-text-*`. No Webflow: selecione o elemento que já tem a classe de texto e **adicione** a segunda classe.

```css
/* Alinhamento */
.u-text-display.is--center,
.u-text-h1.is--center,
.u-text-h2.is--center,
.u-text-h3.is--center,
.u-text-body.is--center {
  text-align: center;
}

/* Texto secundário/apagado */
.u-text-body.is--muted,
.u-text-body-lg.is--muted,
.u-text-sm.is--muted {
  color: var(--semantic-text-muted);
}

/* Cor de marca (destaque) */
.u-text-display.is--brand,
.u-text-h1.is--brand,
.u-text-h2.is--brand {
  color: var(--semantic-brand);
}

/* Texto sobre fundo escuro */
.u-text-h1.is--inverse,
.u-text-h2.is--inverse,
.u-text-body.is--inverse {
  color: var(--semantic-text-inverse);
}

/* Largura de leitura confortável (~65 caracteres) */
.u-text-body.is--measure,
.u-text-body-lg.is--measure {
  max-width: 65ch;
}
```

---

## 5. Tags HTML vs. classes de estilo

| Responsabilidade | Quem resolve |
|---|---|
| Hierarquia semântica / SEO / acessibilidade | A **tag** (`h1`, `h2`, `p`…) |
| Aparência visual (tamanho, peso, cor) | A **classe** (`.u-text-*`) |

Um `<h2>` pode receber `.u-text-h1` quando o **visual** precisa ser grande, mas a semântica continua correta. Exemplo:

```html
<h1 class="u-text-display">Conversas em escala</h1>
<p class="u-text-body-lg is--muted is--measure">Plataforma de WhatsApp Business...</p>
<h2 class="u-text-h2 is--center">Como funciona</h2>
<span class="u-text-caption">Novidade</span>
```

**Regra:** um único `<h1>` por página; headings não pulam níveis (não vá de `h2` para `h4`).

---

## 6. Regras de uso

1. **Nunca** estilize texto na tag base (All H1). Sempre via `.u-text-*`.
2. Tag = semântica, classe = visual.
3. Cor sempre via `semantic/text*`, nunca hex cru.
4. Espaçamento entre textos = `gap`/`l-stack`, não margin na classe.
5. Pesos carregados: 400 / 500 / 700 apenas.
6. Texto corrido longo: `.is--measure` ou container `l-container.is--narrow`.

---

**Anterior:** [← 01 — Tokens](01-tokens.md) | **Próximo:** [03 — Espaçamento & Layout →](03-espacamento-layout.md)
