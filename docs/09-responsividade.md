# 09 — Responsividade

> Como o site se adapta entre telas. No Webflow a responsividade tem regras próprias por causa da **cascata de breakpoints** — entender isso evita retrabalho.

---

## Sumário

1. [Os breakpoints do Webflow](#1-os-breakpoints-do-webflow)
2. [A cascata: desktop-first vs. mobile-first](#2-a-cascata-desktop-first-vs-mobile-first)
3. [Estratégia adotada no projeto](#3-estratégia-adotada-no-projeto)
4. [Reduzir trabalho com tokens fluidos](#4-reduzir-trabalho-com-tokens-fluidos)
5. [Checklist por componente](#5-checklist-por-componente)
6. [Regras de ouro](#6-regras-de-ouro)

---

## 1. Os breakpoints do Webflow

O Webflow tem 6 breakpoints. O **Base (Desktop)** é o ponto de partida; os outros herdam dele.

| Breakpoint | Largura | Direção da cascata |
|---|---|---|
| Desktop XL | ≥ 1920px | Maior — só herda para cima |
| **Desktop (Base)** | 992px – 1919px | **Ponto de partida dos estilos** |
| Tablet | ≤ 991px | Herda do Desktop, propaga p/ baixo |
| Mobile Landscape | ≤ 767px | Herda do Tablet |
| Mobile Portrait | ≤ 478px | Herda do Mobile Landscape |
| Desktop XL (1280/1440 opcionais) | — | — |

---

## 2. A cascata: desktop-first vs. mobile-first

Esta é a regra que mais confunde no Webflow:

- **Para baixo do Base (Tablet → Mobile):** os estilos **herdam em cascata**. Mudou no Tablet → afeta Mobile também (salvo override). É **desktop-first**.
- **Para cima do Base (Desktop XL ≥1920):** **não** herda para baixo. Ajustes lá ficam isolados no XL.

Tradução prática: **estilize primeiro no Desktop (Base), depois desça** ajustando Tablet → Mobile Landscape → Mobile Portrait. Um ajuste no Tablet cobre os menores automaticamente; você só toca nos menores quando precisa de algo específico.

---

## 3. Estratégia adotada no projeto

**Desktop-first, descendo em cascata** (alinhado ao funcionamento natural do Webflow).

Fluxo de trabalho ao montar um componente/seção:

1. Construa e estilize **100% no Desktop (Base)**.
2. Vá para **Tablet**: ajuste o que quebra (grids viram menos colunas, fontes via fluid já se ajustam sozinhas).
3. **Mobile Landscape**: empilhe o que ainda estiver lado a lado.
4. **Mobile Portrait**: ajustes finos (botões `is--full`, espaçamentos menores).

> Padrão de grid: `.l-grid.is--auto` (ver [03](03-espacamento-layout.md)) já quebra colunas sozinho — em muitos casos você não precisa tocar em nada no mobile.

### Conversões típicas mobile

| Desktop | Mobile |
|---|---|
| `.l-grid` 3-4 colunas | 1 coluna |
| `.c-hero` lado-a-lado (content + media) | empilhado, media embaixo |
| `.c-nav__menu` horizontal | menu hambúrguer (`.c-nav__toggle`) |
| `.c-button` inline | `.c-button.is--full` |
| Section padding `space/3xl` | `space/xl` (via fluid ou override) |

---

## 4. Reduzir trabalho com tokens fluidos

A melhor responsividade é a que você **não precisa configurar** em cada breakpoint:

- **Tipografia:** `clamp()` nos `font/size/*` (ver [02-tipografia](02-tipografia.md)) — o texto escala sozinho.
- **Espaçamento de seção:** pode usar `clamp()` em `space/3xl` para o padding vertical encolher em telas pequenas sem override.
- **Grid auto-fit:** `.l-grid.is--auto` quebra colunas pela largura disponível.

Quanto mais você resolve via token fluido, menos overrides por breakpoint — e menos chance de inconsistência entre páginas.

---

## 5. Checklist por componente

Ao finalizar um componente, valide em **todos** os breakpoints:

- [ ] Desktop (Base): visual final correto
- [ ] Tablet: nada estoura largura, grids reduzidos
- [ ] Mobile Landscape: tudo que era lado-a-lado empilhou
- [ ] Mobile Portrait (478): texto legível, botões alcançáveis, sem scroll horizontal
- [ ] Touch targets ≥ 44×44px (links/botões em mobile)
- [ ] Imagens não estouram o container (`max-width: 100%`)

---

## 6. Regras de ouro

1. **Estilize no Desktop primeiro**, depois desça. Nunca comece pelo mobile no Webflow.
2. **Ajuste no maior breakpoint que resolve** — mexer no Tablet cobre os menores. Não repita o mesmo ajuste em 3 breakpoints.
3. **`overflow-x` nunca.** Scroll horizontal em mobile = bug. `.l-page { overflow: hidden }` ajuda, mas ache a causa.
4. **Prefira tokens fluidos** (`clamp()`) a overrides manuais.
5. Teste em **device real** ou no preview do Webflow, não só redimensionando a janela.

---

**Anterior:** [← 08 — Catálogo de Combo Classes](08-combo-classes.md) | **Próximo:** [10 — Performance →](10-performance.md)
