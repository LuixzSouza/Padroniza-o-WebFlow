# 05 — Formulários

> Inputs, selects, checkboxes, validação. **CSS completo de cada classe.** Namespace de componente `c-` para o form e seus elements; campos usam `c-field`/`c-input`.

No Webflow, formulários vêm de um **Form Block** (com `<form>`, e botão de submit). Estilize os campos com as classes abaixo em vez dos estilos default do Webflow.

---

## Sumário

1. [Estrutura de um formulário](#1-estrutura-de-um-formulário)
2. [c-form (wrapper)](#2-c-form-wrapper)
3. [c-field (grupo label + input)](#3-c-field-grupo-label--input)
4. [c-label](#4-c-label)
5. [c-input (text, email, tel, etc.)](#5-c-input)
6. [c-textarea](#6-c-textarea)
7. [c-select](#7-c-select)
8. [c-checkbox e c-radio](#8-c-checkbox-e-c-radio)
9. [Mensagens de validação](#9-mensagens-de-validação)
10. [Estados de sucesso / erro do Webflow](#10-estados-de-sucesso--erro-do-webflow)
11. [Acessibilidade](#11-acessibilidade)

---

## 1. Estrutura de um formulário

```html
<div class="c-form">
  <form>
    <div class="c-field">
      <label class="c-label" for="nome">Nome</label>
      <input class="c-input" type="text" id="nome" name="nome" placeholder="Seu nome">
    </div>

    <div class="c-field">
      <label class="c-label" for="email">Email <span class="c-label__req">*</span></label>
      <input class="c-input" type="email" id="email" name="email" required>
      <span class="c-field__hint">Não compartilhamos seu email.</span>
    </div>

    <div class="c-field">
      <label class="c-label" for="msg">Mensagem</label>
      <textarea class="c-textarea" id="msg" name="msg"></textarea>
    </div>

    <label class="c-checkbox">
      <input type="checkbox" name="aceite">
      <span class="c-checkbox__box"></span>
      <span class="c-checkbox__label u-text-sm">Aceito os termos</span>
    </label>

    <button class="c-button is--primary is--full" type="submit">Enviar</button>
  </form>
</div>
```

---

## 2. c-form (wrapper)

```css
.c-form {
  width: 100%;
  max-width: 480px;
}

/* O <form> dentro: empilha os campos com gap */
.c-form form {
  display: flex;
  flex-direction: column;
  gap: var(--space-md);   /* 24px entre campos */
}
```

### Modifier

```css
/* Formulário em largura cheia (ex.: contato em 2 colunas) */
.c-form.is--full { max-width: none; }
```

---

## 3. c-field (grupo label + input)

```css
.c-field {
  display: flex;
  flex-direction: column;
  gap: var(--space-3xs);   /* 4px entre label e input */
}

.c-field__hint {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 0.8125rem;    /* 13px */
  line-height: 1.4;
  color: var(--semantic-text-muted);
}

/* Layout lado-a-lado (ex.: nome + sobrenome) */
.c-field.is--inline {
  flex-direction: row;
  align-items: center;
  gap: var(--space-sm);
}
```

---

## 4. c-label

```css
.c-label {
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 0.875rem;     /* 14px */
  font-weight: 500;
  line-height: 1.4;
  color: var(--semantic-text);
}

.c-label__req {
  color: var(--semantic-error);   /* asterisco de obrigatório */
}
```

---

## 5. c-input

Cobre `type="text"`, `email`, `tel`, `number`, `url`, `password`, etc.

```css
.c-input {
  width: 100%;
  padding: 12px 16px;
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 1rem;         /* 16px — evita zoom no iOS */
  line-height: 1.4;
  color: var(--semantic-text);
  background-color: var(--semantic-surface);
  border: 1px solid var(--semantic-border);
  border-radius: var(--radius-sm);   /* 4px */
  transition: border-color var(--transition-base) var(--ease-standard),
              box-shadow var(--transition-base) var(--ease-standard);
}

.c-input::placeholder {
  color: var(--semantic-text-muted);
}

.c-input:hover {
  border-color: var(--color-neutral-3);
}

.c-input:focus {
  outline: none;
  border-color: var(--semantic-brand);
  box-shadow: 0 0 0 3px var(--color-purple-1);   /* anel de foco */
}

.c-input:disabled {
  background-color: var(--semantic-surface-alt);
  color: var(--semantic-text-muted);
  cursor: not-allowed;
}
```

### Modifier de erro

```css
.c-input.is--error {
  border-color: var(--semantic-error);
}
.c-input.is--error:focus {
  box-shadow: 0 0 0 3px var(--color-red-1);
}
```

---

## 6. c-textarea

Mesma base do input, com altura e resize.

```css
.c-textarea {
  width: 100%;
  min-height: 120px;
  padding: 12px 16px;
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 1rem;
  line-height: 1.5;
  color: var(--semantic-text);
  background-color: var(--semantic-surface);
  border: 1px solid var(--semantic-border);
  border-radius: var(--radius-sm);
  resize: vertical;        /* só redimensiona na vertical */
  transition: border-color var(--transition-base) var(--ease-standard),
              box-shadow var(--transition-base) var(--ease-standard);
}
.c-textarea:focus {
  outline: none;
  border-color: var(--semantic-brand);
  box-shadow: 0 0 0 3px var(--color-purple-1);
}
```

---

## 7. c-select

```css
.c-select {
  width: 100%;
  padding: 12px 40px 12px 16px;   /* espaço extra à direita p/ a seta */
  font-family: "DM Sans", Helvetica, Arial, sans-serif;
  font-size: 1rem;
  line-height: 1.4;
  color: var(--semantic-text);
  background-color: var(--semantic-surface);
  border: 1px solid var(--semantic-border);
  border-radius: var(--radius-sm);
  cursor: pointer;
  appearance: none;        /* remove a seta nativa p/ usar a custom */

  /* seta custom em SVG inline (roxo) */
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath fill='%23460095' d='M6 8 0 2 1.4.6 6 5.2 10.6.6 12 2z'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 16px center;
  transition: border-color var(--transition-base) var(--ease-standard);
}
.c-select:focus {
  outline: none;
  border-color: var(--semantic-brand);
  box-shadow: 0 0 0 3px var(--color-purple-1);
}
```

> No Webflow use um **Select Element**. A seta nativa varia por SO — `appearance: none` + SVG de fundo garante consistência.

---

## 8. c-checkbox e c-radio

O input nativo é escondido e um box custom é estilizado. Wrapper é a própria `<label>` para a área clicável incluir o texto.

### Checkbox

```css
.c-checkbox {
  display: inline-flex;
  align-items: flex-start;
  gap: var(--space-2xs);   /* 8px box ↔ texto */
  cursor: pointer;
}

/* Esconde o input nativo (mas mantém acessível) */
.c-checkbox input[type="checkbox"] {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.c-checkbox__box {
  flex-shrink: 0;
  width: 20px;
  height: 20px;
  margin-top: 2px;
  background-color: var(--semantic-surface);
  border: 1px solid var(--semantic-border);
  border-radius: var(--radius-sm);
  transition: background-color var(--transition-base) var(--ease-standard),
              border-color var(--transition-base) var(--ease-standard);
}

/* Marcado */
.c-checkbox input:checked ~ .c-checkbox__box {
  background-color: var(--semantic-brand);
  border-color: var(--semantic-brand);
  /* tique branco em SVG */
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='10' viewBox='0 0 12 10'%3E%3Cpath fill='none' stroke='%23fff' stroke-width='2' d='M1 5l3 3 7-7'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: center;
}

/* Foco (acessibilidade via teclado) */
.c-checkbox input:focus-visible ~ .c-checkbox__box {
  box-shadow: 0 0 0 3px var(--color-purple-1);
}

.c-checkbox__label {
  color: var(--semantic-text);
}
```

### Radio

Mesma lógica, mas `border-radius: var(--radius-full)` e o "marcado" é um ponto central.

```css
.c-radio {
  display: inline-flex;
  align-items: flex-start;
  gap: var(--space-2xs);
  cursor: pointer;
}
.c-radio input[type="radio"] {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}
.c-radio__dot {
  flex-shrink: 0;
  width: 20px;
  height: 20px;
  margin-top: 2px;
  background-color: var(--semantic-surface);
  border: 1px solid var(--semantic-border);
  border-radius: var(--radius-full);   /* círculo */
  transition: border-color var(--transition-base) var(--ease-standard);
}
.c-radio input:checked ~ .c-radio__dot {
  border-color: var(--semantic-brand);
  border-width: 6px;        /* o "preenchimento" vem da borda grossa */
}
.c-radio input:focus-visible ~ .c-radio__dot {
  box-shadow: 0 0 0 3px var(--color-purple-1);
}
```

---

## 9. Mensagens de validação

```css
.c-field__error {
  display: flex;
  align-items: center;
  gap: var(--space-3xs);
  font-size: 0.8125rem;     /* 13px */
  line-height: 1.4;
  color: var(--semantic-error);
}
.c-field__error svg { width: 14px; height: 14px; }
```

Mostre/esconda via interação Webflow ou validação custom. Combine com `.c-input.is--error`.

---

## 10. Estados de sucesso / erro do Webflow

O Form Block do Webflow já gera dois blocos: **Success** e **Error**. Estilize-os:

```css
/* Mensagem de sucesso (após envio) */
.c-form .w-form-done {
  display: flex;
  align-items: center;
  gap: var(--space-2xs);
  padding: var(--space-sm);
  background-color: var(--color-green-1);   /* #d2f2e3 */
  color: var(--color-green-4-5);
  border-radius: var(--radius-md);
}

/* Mensagem de erro de envio */
.c-form .w-form-fail {
  display: flex;
  align-items: center;
  gap: var(--space-2xs);
  padding: var(--space-sm);
  background-color: var(--color-red-1);     /* #ffe1d8 */
  color: var(--color-red-4-5);
  border-radius: var(--radius-md);
}
```

> `.w-form-done` e `.w-form-fail` são classes nativas do Webflow — você adiciona uma combo `is--` ou estiliza dentro de `.c-form` como acima.

---

## 11. Acessibilidade

1. **Todo input tem `<label>`** com `for` apontando para o `id`. Placeholder **não** substitui label.
2. **Foco visível** sempre (`:focus`/`:focus-visible` com anel) — nunca `outline: none` sem substituir por outra indicação.
3. `font-size` mínimo **16px** em inputs evita zoom automático no iOS.
4. Campos obrigatórios: `required` no HTML + indicação visual (`c-label__req`).
5. Mensagens de erro associadas ao campo (`aria-describedby`).
6. Área clicável de checkbox/radio inclui o texto (por isso o wrapper é `<label>`).

---

**Anterior:** [← 04 — Componentes](04-componentes.md) | **Próximo:** [06 — Utilitários →](06-utilitarios.md)
