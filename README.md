# BEM no Webflow — Guia de Padronização (SOP)

> **Standard Operating Procedure** para nomenclatura de classes CSS em projetos Webflow utilizando a metodologia **BEM (Block, Element, Modifier)**.
>
> Documento técnico de referência para a equipe de desenvolvimento — baseado nas diretrizes oficiais do artigo [*Class naming 101: BEM* (Webflow Blog)](https://webflow.com/blog/class-naming-101-bem).

---

## Sumário

1. [Por que BEM no Webflow](#1-por-que-bem-no-webflow)
2. [Anatomia BEM](#2-anatomia-bem)
3. [Block (Bloco)](#3-block-bloco)
4. [Element (Elemento)](#4-element-elemento)
5. [Modifier (Modificador)](#5-modifier-modificador)
6. [Global Classes vs. Combo Classes no Webflow](#6-global-classes-vs-combo-classes-no-webflow)
7. [Regras de Ouro](#7-regras-de-ouro)
8. [Exemplo Prático — Seção Hero](#8-exemplo-prático--seção-hero)
9. [Cheat Sheet](#9-cheat-sheet)
10. [Apêndice: Namespaces estendidos](#10-apêndice-namespaces-estendidos)

---

## 1. Por que BEM no Webflow

CSS sem convenção vira CSS não manutenível. Em projetos Webflow isso é ainda mais crítico porque:

- O **CSS é gerado pelo Designer** — não há refactor "no editor de código". Nomes ruins ficam ruins para sempre.
- O **Cross-site copy/paste** depende de classes que façam sentido fora do contexto original. Se você copia um `.hero-2` para outro projeto, ninguém sabe o que ele faz.
- As **interações do Webflow são acopladas à classe global**. Renomear classe quebra animação; criar variações sem padrão obriga a duplicar animações.

BEM resolve isso ao impor três regras simples: cada classe declara explicitamente se é um **componente autônomo (Block)**, uma **parte interna desse componente (Element)** ou uma **variação visual (Modifier)**. O resultado é uma arquitetura modular onde qualquer pessoa olhando para `.c-hero__title.is--large` entende instantaneamente:

- É um componente (`c-`)
- Chamado `hero`
- O elemento interno `title`
- Em sua variação `large`

### Comparação com Tailwind

Você mencionou gostar de Tailwind. A intuição é boa, mas o paradigma é diferente:

| Tailwind | BEM no Webflow |
|---|---|
| Utility-first: cada classe = uma propriedade CSS | Component-first: cada classe = um componente lógico |
| `class="flex gap-4 p-6 bg-white"` no HTML | `.c-hero` no Designer, estilos aplicados via painel |
| Sem nomes semânticos | Nomes semânticos obrigatórios |
| Compose direto no markup | Compose via **Combo Classes** (`.c-hero` + `.is--dark`) |

A "essência" Tailwind no Webflow vem das **Combo Classes**: você não consegue (e não deveria) empilhar 10 utilities num elemento, mas pode compor uma classe base com um modifier para criar variantes — exatamente como `bg-white` vs. `bg-white dark:bg-black`, só que no padrão `.c-card` + `.is--dark`.

---

## 2. Anatomia BEM

```
.c-block__element.is--modifier
   │      │         │
   │      │         └── Modifier  (variação)        — combo class
   │      └──────────── Element   (parte filha)     — global class
   └─────────────────── Block     (componente raiz) — global class
```

**Regras de sintaxe (não negociáveis):**

- Prefixo de namespace: `c-` para componente
- Separador Block → Element: **dois underlines** `__`
- Separador palavras dentro de um nome: **um hífen** `-`
- Modifier: classe combinada **separada**, com prefixo `is--`
- Tudo em **lowercase**

---

## 3. Block (Bloco)

Um **Block** é um componente autônomo de alto nível. Ele faz sentido sozinho, independente do contexto. Exemplos típicos: navegação, hero, footer, card, menu, formulário de newsletter.

### Regra de nomenclatura

```css
.c-[nome-do-bloco] { }
```

O prefixo `c-` indica explicitamente que aquela classe é um **componente**. Esse é o **namespace** — ele te diz, em termos não-relativos, o que aquela classe faz globalmente no projeto.

### Exemplos

```css
.c-nav        { /* navegação principal */ }
.c-hero       { /* seção hero */ }
.c-footer     { /* rodapé */ }
.c-card       { /* card reutilizável */ }
.c-cta        { /* bloco de call-to-action */ }
.c-feature    { /* bloco de feature destacada */ }
```

### Quando NÃO é um Block

- Se o elemento só existe **dentro** de outro componente → é um Element, não um Block.
- Se o elemento é uma **variação** de um Block existente → é um Modifier.
- Se é um wrapper utilitário sem semântica de componente (ex.: container, grid) → considere usar namespace utilitário (`l-` de layout — ver apêndice).

---

## 4. Element (Elemento)

Um **Element** é uma parte filha de um Block. **Não existe fora** do Block ao qual pertence. Um `__title` só tem sentido dentro de `.c-hero` ou `.c-card`.

### Regra de nomenclatura

```css
.c-[bloco]__[elemento] { }
```

Sempre **dois underlines** entre Block e Element. Nunca encadear elementos (`.c-hero__title__subtitle` está **errado** — veja a regra de ouro #4 adiante).

### Exemplos

```css
.c-nav__logo       { }
.c-nav__link       { }
.c-nav__menu       { }

.c-hero__title     { }
.c-hero__subtitle  { }
.c-hero__image     { }
.c-hero__cta       { }

.c-card__header    { }
.c-card__body      { }
.c-card__footer    { }
```

### Por que dois underlines

Visualmente, `__` cria um "respiro" que torna óbvio onde termina o nome do Block e começa o nome do Element. `.c-hero-title` é ambíguo (o Block se chama `hero` ou `hero-title`?); `.c-hero__title` é inequívoco.

---

## 5. Modifier (Modificador)

Um **Modifier** representa uma variação de um Block ou Element — diferença de estilo, estado ou contexto, **sem alterar a identidade** do componente original.

Exemplos: navegação transparente vs. sólida, botão primário vs. secundário, hero com texto grande vs. pequeno.

### A adaptação Webflow: Combo Classes com `is--`

BEM tradicional usa duplo hífen no próprio nome da classe: `.c-button--primary`. **No Webflow isso não funciona bem** porque:

1. Quebra a reutilização de **interações**, que são vinculadas à classe global.
2. Duplica a definição base do componente em cada variação.

A solução é usar uma **Combo Class** com prefixo `is--`:

```css
/* ❌ BEM tradicional — evitar no Webflow */
.c-nav--transparent { }

/* ✅ BEM-Webflow — combo class */
.c-nav.is--transparent { }
```

No Webflow Designer, isso significa: selecione o elemento que já tem `.c-nav`, e **adicione** a classe `.is--transparent`. O Webflow cria a combo class `.c-nav.is--transparent` automaticamente, e qualquer interação configurada em `.c-nav` continua funcionando.

### Aplicação em Blocks

```css
.c-nav.is--transparent     { }
.c-nav.is--dark            { }
.c-nav.is--sticky          { }

.c-card.is--featured       { }
.c-card.is--compact        { }
```

### Aplicação em Elements

```css
.c-hero__text.is--small    { }
.c-hero__text.is--big      { }

.c-button.is--primary      { }
.c-button.is--secondary    { }
.c-button.is--ghost        { }
```

### Por que prefixo `is--`

- `is` comunica **estado/variação** (lê-se "is transparent", "is primary")
- O duplo hífen mantém a herança visual do BEM original
- Cria uma **família** de classes reservadas: qualquer classe iniciada por `.is--` é, por convenção, um modifier
- Facilita buscas globais e auditorias do CSS

---

## 6. Global Classes vs. Combo Classes no Webflow

Esta é a parte que torna BEM-Webflow específico. No Designer, classes funcionam de duas formas:

### Global Classes (estrutura base)

Classes únicas aplicadas a qualquer elemento. Mudou a definição → muda em todos os elementos que a usam.

**Use para:** Blocks, Elements, estrutura, layout base.

```
.c-hero           ← global
.c-hero__title    ← global
.c-hero__cta      ← global
```

### Combo Classes (variações)

Classe adicionada **em cima** de uma global, criando uma variante. Mudou a definição → só muda quando a combinação exata estiver presente.

**Use para:** Modifiers, e somente para modifiers.

```
.c-hero.is--dark          ← combo (modifica .c-hero quando .is--dark está presente)
.c-hero__title.is--large  ← combo
```

### Regra prática no Designer

1. Crie o elemento com a global class (`.c-hero`).
2. Estilize tudo o que é **comum a todas as variações**.
3. Para criar uma variação: selecione o elemento, no painel de classes adicione uma segunda classe (`.is--dark`).
4. Estilize **apenas** o que difere da base. O Webflow vai aplicar esses estilos somente quando a combinação `.c-hero.is--dark` existir.

### O que NÃO fazer

- ❌ Empilhar 3+ classes globais num elemento esperando comportamento Tailwind. Vira "specificity hell".
- ❌ Usar combo class sem uma global como base (`.is--dark` sozinha não significa nada).
- ❌ Criar `.c-hero-dark` como global separada — você está duplicando a base sem necessidade.

---

## 7. Regras de Ouro

1. **Nomes curtos e claros.** `.c-nav` é melhor que `.c-main-navigation-component`. Mas `.c-h` é ruim — clareza ganha de brevidade.
2. **Acrônimos só se a equipe inteira reconhecer.** `.c-cta` (call-to-action) é universal. `.c-fwf` (full-width feature) provavelmente não.
3. **Lowercase sempre.** Sem camelCase, sem PascalCase. `kebab-case` para separação de palavras.
4. **Sem aninhar Elements.** `.c-hero__title__icon` está errado. Se um Element precisa de um filho próprio, ou ele é um Element direto do Block (`.c-hero__title-icon`), ou virou um componente próprio (`.c-icon` reaproveitado).
5. **Um Modifier não vive sozinho.** `is--dark` sempre acompanha uma global class. Modifier "solto" é um cheiro de código mal estruturado.
6. **Block é independente de contexto.** Se você precisa dizer "este hero só funciona dentro do menu", ele não é um Block — provavelmente é um Element.
7. **Pense em copy/paste.** Antes de nomear, pergunte: "se eu colar isso em outro projeto amanhã, esse nome ainda faz sentido?"
8. **Documente acrônimos do projeto.** Mantenha uma seção compartilhada com as siglas adotadas pela equipe.

---

## 8. Exemplo Prático — Seção Hero

Aplicação completa do padrão num componente real: a seção Hero do site.

### Estrutura visual

```
┌─────────────────────────────────────────────────────┐
│  c-hero                                             │
│  ┌───────────────────────────────────────────────┐  │
│  │  c-hero__content                              │  │
│  │                                               │  │
│  │   c-hero__eyebrow      "Novidade"             │  │
│  │   c-hero__title        "Título principal"     │  │
│  │   c-hero__subtitle     "Subtítulo descrit..." │  │
│  │                                               │  │
│  │   c-hero__actions                             │  │
│  │     ├ c-button.is--primary    "Começar"       │  │
│  │     └ c-button.is--ghost      "Saiba mais"    │  │
│  │                                               │  │
│  └───────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────┐  │
│  │  c-hero__media                                │  │
│  │   └ c-hero__image                             │  │
│  └───────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

### Classes (estado base)

```css
/* Block */
.c-hero { }

/* Elements */
.c-hero__content   { }
.c-hero__eyebrow   { }
.c-hero__title     { }
.c-hero__subtitle  { }
.c-hero__actions   { }
.c-hero__media     { }
.c-hero__image     { }

/* Block reutilizado dentro do Hero (não é Element do Hero) */
.c-button { }
```

### Variações (modifiers)

```css
/* Variações do Block — diferentes contextos da página */
.c-hero.is--dark        { /* fundo escuro */ }
.c-hero.is--centered    { /* conteúdo centralizado, sem media lateral */ }
.c-hero.is--compact     { /* versão menor, para páginas internas */ }

/* Variações de Elements */
.c-hero__title.is--large    { /* título maior em landing pages */ }
.c-hero__title.is--small    { /* título menor em sub-páginas */ }

/* Variações do Button (Block reutilizado) */
.c-button.is--primary   { }
.c-button.is--ghost     { }
.c-button.is--small     { }
```

### HTML resultante (simplificado)

```html
<section class="c-hero is--dark">
  <div class="c-hero__content">
    <span class="c-hero__eyebrow">Novidade</span>
    <h1 class="c-hero__title is--large">Conversas em escala para impacto social</h1>
    <p class="c-hero__subtitle">Plataforma de WhatsApp Business para organizações.</p>

    <div class="c-hero__actions">
      <a class="c-button is--primary">Começar agora</a>
      <a class="c-button is--ghost">Falar com vendas</a>
    </div>
  </div>

  <div class="c-hero__media">
    <img class="c-hero__image" src="..." alt="..." />
  </div>
</section>
```

### Leitura para a equipe

Qualquer dev abrindo este HTML em 6 meses entende imediatamente:

- É a seção Hero (`c-hero`)
- Está na variação escura (`is--dark`)
- O título está na variação grande (`is--large`)
- Tem dois botões: um primário, um ghost
- Pode ser copiado para outro projeto Webflow sem ajustes de nomenclatura

---

## 9. Cheat Sheet

**Guia rápido de consulta diária.**

### Sintaxe

| Conceito | Padrão | Exemplo |
|---|---|---|
| Block | `.c-[nome]` | `.c-hero` |
| Element | `.c-[bloco]__[elemento]` | `.c-hero__title` |
| Modifier de Block | `.c-[bloco].is--[var]` | `.c-hero.is--dark` |
| Modifier de Element | `.c-[bloco]__[el].is--[var]` | `.c-hero__title.is--large` |

### Separadores

| Caso | Separador |
|---|---|
| Block → Element | `__` (duplo underline) |
| Palavras dentro de um nome | `-` (hífen único) |
| Classe base → Modifier | espaço (combo class, prefixo `is--`) |

### Checklist antes de nomear uma classe

- [ ] É um componente autônomo? → **Block** (`.c-nome`)
- [ ] É parte interna de um componente? → **Element** (`.c-bloco__parte`)
- [ ] É uma variação visual/estado? → **Modifier** (combo `.is--var`)
- [ ] O nome faz sentido fora do contexto atual?
- [ ] Está em lowercase e kebab-case?
- [ ] Estou usando combo class para o modifier (não global separada)?

### Decisão rápida: Global ou Combo?

```
Esta classe define a estrutura base?           → Global
Esta classe modifica uma estrutura existente?  → Combo (is--*)
```

### Anti-padrões a evitar

| ❌ Errado | ✅ Correto | Por quê |
|---|---|---|
| `.hero1`, `.hero2` | `.c-hero.is--landing`, `.c-hero.is--internal` | Numeração não comunica intenção |
| `.c-Hero__Title` | `.c-hero__title` | Sempre lowercase |
| `.c-hero_title` | `.c-hero__title` | Element exige `__`, não `_` |
| `.c-hero__title__icon` | `.c-hero__title-icon` | Não aninhar elements |
| `.c-hero--dark` | `.c-hero.is--dark` | No Webflow, modifier é combo |
| `.is--dark` (sozinha) | `.c-hero.is--dark` | Modifier exige classe base |
| `.c-nav .c-menu .c-link` empilhadas | Compor com combo classes | Specificity hell |

---

## 10. Apêndice: Namespaces estendidos

O prefixo `c-` cobre **componentes**, que é 90% do que você vai nomear. Para projetos maiores, considere ampliar:

| Prefixo | Significado | Exemplo |
|---|---|---|
| `c-` | Component | `.c-hero`, `.c-card` |
| `l-` | Layout / Container | `.l-container`, `.l-grid` |
| `u-` | Utility (uso pontual, single-purpose) | `.u-hidden`, `.u-sr-only` |
| `is--` | State / Modifier | `.is--active`, `.is--dark` |
| `has-` | State indicando que o elemento possui algo | `.has-error`, `.has-icon` |
| `js-` | Hook para JavaScript (não estiliza) | `.js-modal-trigger` |

**Importante:** comece com `c-` e `is--`. Só adicione os outros namespaces quando o projeto justificar. Convenção em excesso vira burocracia.

---

## Referência

- Artigo oficial: [Class naming 101: BEM — Webflow Blog](https://webflow.com/blog/class-naming-101-bem)
- Metodologia BEM original: [getbem.com](https://getbem.com)

---

*Documento mantido pela equipe de desenvolvimento. Atualize quando convenções do projeto evoluírem.*