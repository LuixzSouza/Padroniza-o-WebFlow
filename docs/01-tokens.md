# 01 — Design Tokens (Webflow Variables)

> A camada **base** do style guide. Tokens são os valores brutos do design (cores, espaçamentos, tamanhos) definidos **uma vez** como **Webflow Variables** e reutilizados em todo o projeto. Mudou o token → mudou em todo lugar.

---

## Sumário

1. [O que são tokens e por que usar Variables](#1-o-que-são-tokens-e-por-que-usar-variables)
2. [Cores](#2-cores)
3. [Cores semânticas (aliases)](#3-cores-semânticas-aliases)
4. [Tipografia (tokens)](#4-tipografia-tokens)
5. [Espaçamento](#5-espaçamento)
6. [Border radius](#6-border-radius)
7. [Sombras](#7-sombras)
8. [Border / linhas](#8-border--linhas)
9. [Transições / easing](#9-transições--easing)
10. [Z-index (camadas)](#10-z-index-camadas)
11. [Breakpoints](#11-breakpoints)
12. [Larguras de container](#12-larguras-de-container)
13. [Como criar isso no Webflow Designer](#13-como-criar-isso-no-webflow-designer)

---

## 1. O que são tokens e por que usar Variables

Um **token** é uma decisão de design transformada em variável nomeada. Em vez de digitar `#460095` em 60 lugares, você cria a Variable `color/brand/primary` e usa ela. No Webflow isso vive em **Variables** (painel de Variáveis, dentro de uma *Variable Collection*).

**Por que isso é inegociável no projeto novo:**

- **Consistência:** ninguém "chuta" um roxo levemente diferente. Existe um só.
- **Tema/manutenção:** trocar a cor de marca é editar 1 variável, não caçar hex pelo site.
- **Modo claro/escuro e responsivo:** Variables podem ter valores diferentes por modo e por breakpoint.
- **Onboarding:** a pessoa nova escolhe de uma lista nomeada em vez de adivinhar.

> Regra de ouro: **nenhum valor cru de cor/spacing direto no estilo de um elemento.** Sempre uma Variable.

> ⚠️ **Sobre os nomes `var(--...)` nesta doc:** ao longo dos arquivos eu escrevo `var(--semantic-brand)`, `var(--space-md)`, etc. **Você não digita isso à mão no Webflow** — você seleciona a Variable na interface (seletor de cor, campo de tamanho). O Webflow **gera o nome do CSS custom property automaticamente** a partir do nome da Variable, e o nome exato pode variar (ex.: `--color--purple-5`). Trate os `var(--...)` da doc como **referência de qual Variable escolher**, não como texto literal a copiar. O que importa é: criou a Variable com o nome certo → escolheu ela no painel. Use `var(...)` literal só dentro de **custom code** ou ao definir uma Variable que aponta para outra.

---

## 2. Cores

Estes valores foram **extraídos do turn.io atual** (CSS do Webflow). O site já segue uma escala numérica `1 → 5` por família, onde **1 = mais claro** e **5 = mais escuro** (`4-5` é um passo intermediário escuro). Vamos manter essa escala porque já está estabelecida e é boa.

### Estrutura no Webflow

Collection: **`Cores`** → grupos por família. Naming sugerido das Variables: `color/[família]/[tom]`.

### Roxo (Purple) — cor de marca principal

> `purple/5` (`#460095`) é a cor **mais usada** no site atual (aparece 63×). É a cor de marca.

| Variable | Hex | Uso típico |
|---|---|---|
| `color/purple/1` | `#eee2ff` | Fundos suaves, tags |
| `color/purple/2` | `#d7c0ff` | Hover de fundo claro |
| `color/purple/3` | `#c09dff` | Detalhes, bordas |
| `color/purple/4` | `#945ae3` | Ações secundárias |
| `color/purple/4-5` | `#6d2dbc` | Hover de botão primário |
| `color/purple/5` | `#460095` | **Marca / botão primário / texto destaque** |

### Azul (Blue)

| Variable | Hex |
|---|---|
| `color/blue/1` | `#ccf0f8` |
| `color/blue/2` | `#66d2ea` |
| `color/blue/3` | `#00b4dc` |
| `color/blue/4` | `#0082a0` |
| `color/blue/4-5` | `#005c71` |
| `color/blue/5` | `#003642` |

### Verde (Green) — sucesso / positivo

| Variable | Hex |
|---|---|
| `color/green/1` | `#d2f2e3` |
| `color/green/2` | `#77d9ab` |
| `color/green/3` | `#1dbf73` |
| `color/green/4` | `#189258` |
| `color/green/4-5` | `#11663d` |
| `color/green/5` | `#0a3922` |

### Rosa (Pink)

| Variable | Hex |
|---|---|
| `color/pink/1` | `#fce6ff` |
| `color/pink/2` | `#f3b3fe` |
| `color/pink/3` | `#eb80fd` |
| `color/pink/4` | `#ab5cb6` |
| `color/pink/4-5` | `#794181` |
| `color/pink/5` | `#47264c` |

### Vermelho (Red) — erro / alerta

| Variable | Hex |
|---|---|
| `color/red/1` | `#ffe1d8` |
| `color/red/2` | `#fea689` |
| `color/red/3` | `#ff643b` |
| `color/red/4` | `#bc522f` |
| `color/red/4-5` | `#843920` |
| `color/red/5` | `#4c2011` |

### Amarelo (Yellow) — aviso / destaque

| Variable | Hex |
|---|---|
| `color/yellow/1` | `#f7f2d8` |
| `color/yellow/2` | `#e7d88a` |
| `color/yellow/3` | `#d8c833` |
| `color/yellow/4` | `#99872a` |
| `color/yellow/4-5` | `#6d601e` |
| `color/yellow/5` | `#403911` |

### Neutros (Black scale) — texto, fundos, bordas

> Atenção: no site atual a família se chama `black` mas `black/1` é cinza **claro**. Recomendo renomear para `neutral` no projeto novo (ver nota abaixo).

| Variable | Hex |
|---|---|
| `color/neutral/1` | `#f2f2f2` |
| `color/neutral/2` | `#e1e1e1` |
| `color/neutral/3` | `#a6a6a6` |
| `color/neutral/4` | `#7a7a7a` |
| `color/neutral/4-5` | `#3d3d3d` |
| `color/neutral/5` | `#111111` *(definir o preto final — usar `#000000` apenas se necessário)* |
| `color/white` | `#ffffff` |

> **Decisão pendente para a equipe:** manter o nome `black/*` (compatível com o site atual) ou migrar para `neutral/*` (mais correto). Recomendo `neutral/*` no rebuild — anota a escolha no [11-fluxo-de-trabalho](11-fluxo-de-trabalho.md).

---

## 3. Cores semânticas (aliases)

Aqui está o pulo do gato que falta no site atual: **não use `purple/5` direto nos componentes.** Crie uma segunda camada de Variables **semânticas** que apontam para as primitivas. Assim você troca o significado sem mexer na paleta.

Collection sugerida: **`Semantic`** (ou um grupo `semantic/` na mesma collection).

| Variable semântica | Aponta para | Uso |
|---|---|---|
| `semantic/brand` | `color/purple/5` | Cor primária da marca |
| `semantic/brand-hover` | `color/purple/4-5` | Hover de elementos de marca |
| `semantic/text` | `color/neutral/4-5` | Texto de corpo padrão |
| `semantic/text-muted` | `color/neutral/4` | Texto secundário |
| `semantic/text-inverse` | `color/white` | Texto sobre fundo escuro |
| `semantic/surface` | `color/white` | Fundo de páginas/cards |
| `semantic/surface-alt` | `color/neutral/1` | Fundo de seção alternada |
| `semantic/border` | `color/neutral/2` | Bordas, divisores |
| `semantic/success` | `color/green/3` | Estados positivos |
| `semantic/error` | `color/red/3` | Estados de erro |
| `semantic/warning` | `color/yellow/3` | Avisos |

**Regra prática:** componentes (`.c-*`) usam **semantic/**. Tokens primitivos (`color/purple/5`) só aparecem na definição das variáveis semânticas. Isso permite trocar tema (ex.: dark mode) editando os aliases.

---

## 4. Tipografia (tokens)

Fonte oficial: **DM Sans** (já em uso no site, carregada via Google Fonts). Detalhes de escala e text styles em [02-tipografia](02-tipografia.md). Aqui só os tokens base.

| Variable | Valor | Uso |
|---|---|---|
| `font/family/base` | `"DM Sans", Helvetica, Arial, sans-serif` | Tudo |
| `font/weight/regular` | `400` | Corpo |
| `font/weight/medium` | `500` | Ênfase leve |
| `font/weight/bold` | `700` | Headings |
| `font/lh/tight` | `1.1` | Headings grandes |
| `font/lh/base` | `1.5` | Corpo |

---

## 5. Espaçamento

Escala de espaçamento consistente (margin, padding, gap). Base **rem** (1rem = 16px). Use uma escala fixa — nada de "23px no olho".

Collection: **`Spacing`** → `space/[tamanho]`.

| Variable | rem | px | Uso |
|---|---|---|---|
| `space/0` | 0 | 0 | Reset |
| `space/3xs` | 0.25 | 4 | Gaps mínimos (ícone+texto) |
| `space/2xs` | 0.5 | 8 | Entre elementos próximos |
| `space/xs` | 0.75 | 12 | — |
| `space/sm` | 1 | 16 | Padding base |
| `space/md` | 1.5 | 24 | — |
| `space/lg` | 2 | 32 | Entre blocos |
| `space/xl` | 3 | 48 | Espaço interno de seções pequenas |
| `space/2xl` | 4 | 64 | — |
| `space/3xl` | 6 | 96 | Padding vertical de seções |
| `space/4xl` | 8 | 128 | Seções grandes |

> Como aplicar no layout (section padding, gaps, container) está em [03-espacamento-layout](03-espacamento-layout.md).

---

## 6. Border radius

| Variable | Valor | Uso |
|---|---|---|
| `radius/none` | 0 | — |
| `radius/sm` | 4px | Inputs, tags |
| `radius/md` | 8px | Botões, cards |
| `radius/lg` | 16px | Cards grandes, modais |
| `radius/full` | 9999px | Pills, avatares |

---

## 7. Sombras

| Variable | Valor sugerido | Uso |
|---|---|---|
| `shadow/sm` | `0 1px 2px rgba(0,0,0,.06)` | Hover sutil |
| `shadow/md` | `0 4px 12px rgba(0,0,0,.08)` | Cards |
| `shadow/lg` | `0 12px 32px rgba(0,0,0,.12)` | Modais, dropdowns |

> Sombras pesadas custam pintura/render. Use com parcimônia (ver [10-performance](10-performance.md)).

---

## 8. Border / linhas

| Variable | Valor | Uso |
|---|---|---|
| `border/width/sm` | 1px | Bordas padrão, divisores |
| `border/width/md` | 2px | Foco, destaque |
| `border/color` | `var(--semantic-border)` | Cor de borda padrão |

---

## 9. Transições / easing

Padronize a velocidade e a curva das animações para o site ter um "feel" coeso.

| Variable | Valor | Uso |
|---|---|---|
| `transition/fast` | `120ms` | Toques, active |
| `transition/base` | `200ms` | Hover de botões/links (padrão) |
| `transition/slow` | `320ms` | Aberturas (accordion, dropdown) |
| `ease/standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | Curva padrão |
| `ease/out` | `cubic-bezier(0, 0, 0.2, 1)` | Entradas |
| `ease/in` | `cubic-bezier(0.4, 0, 1, 1)` | Saídas |

Uso típico: `transition: background-color var(--transition-base) var(--ease-standard);`

> **Performance:** anime só `transform` e `opacity` (ver [10-performance](10-performance.md)).

---

## 10. Z-index (camadas)

Escala fixa de empilhamento — acaba com a guerra de `z-index: 9999`.

| Variable | Valor | Uso |
|---|---|---|
| `z/base` | 0 | Conteúdo normal |
| `z/raised` | 10 | Cards em hover, elementos elevados |
| `z/sticky` | 100 | Nav sticky |
| `z/dropdown` | 200 | Dropdowns, tooltips |
| `z/overlay` | 900 | Fundo escurecido de modal |
| `z/modal` | 1000 | Modal/dialog |
| `z/toast` | 1100 | Notificações no topo de tudo |

> Regra: **nunca** use um z-index fora desta escala. Precisa de algo "acima do modal"? Adicione um token novo aqui, não um número mágico.

---

## 11. Breakpoints

São os breakpoints nativos do Webflow (não-editáveis na maioria). Documentados aqui como referência para `clamp()`/media queries em custom code. Detalhe de estratégia em [09-responsividade](09-responsividade.md).

| Nome | Largura | Token (p/ custom code) |
|---|---|---|
| Desktop XL | ≥ 1920px | `bp/xl: 1920px` |
| Desktop (base) | 992–1919px | — (ponto de partida) |
| Tablet | ≤ 991px | `bp/tablet: 991px` |
| Mobile Landscape | ≤ 767px | `bp/mobile-l: 767px` |
| Mobile Portrait | ≤ 478px | `bp/mobile-p: 478px` |

---

## 12. Larguras de container

Centralizadas para casar com [03-espacamento-layout](03-espacamento-layout.md).

| Variable | Valor | Uso |
|---|---|---|
| `container/narrow` | 760px | Texto longo, blog, legal |
| `container/base` | 1200px | Padrão |
| `container/wide` | 1400px | Grids largos, galerias |

---

## 13. Como criar isso no Webflow Designer

1. Abra o painel **Variables** (ícone de variável na barra direita, ou `Style Panel → Variables`).
2. Crie uma **Variable Collection** chamada `Core` (ou separe em `Cores`, `Spacing`, `Type`).
3. Para cada token, **New Variable** → escolha o tipo (Color, Size, Font, etc.).
4. Use **`/` no nome** para agrupar: digitar `color/purple/5` cria a hierarquia visual de pastas automaticamente.
5. Crie as **Variables semânticas** apontando para as primitivas: ao definir o valor, escolha **"Variable"** como fonte e selecione `color/purple/5`.
6. Nos componentes, **sempre** escolha a variável no seletor de cor — nunca digite hex.

> **Modo escuro (opcional):** na Variable Collection, crie um **Mode** "Dark" e redefina só as `semantic/*`. Componentes herdam automaticamente.

---

**Anterior:** [← 00 — Nomenclatura](00-nomenclatura.md) | **Próximo:** [02 — Tipografia →](02-tipografia.md)
