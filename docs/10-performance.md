# 10 — Performance

> **Este é o motivo do rebuild.** O site atual tem PageSpeed ruim. Aqui ficam as regras para não repetir os erros. Performance não é "otimizar no fim" — é decisão de construção desde o primeiro dia.

---

## Sumário

1. [As métricas que importam (Core Web Vitals)](#1-as-métricas-que-importam-core-web-vitals)
2. [Imagens — o maior ofensor](#2-imagens--o-maior-ofensor)
3. [Fontes](#3-fontes)
4. [Custom code e scripts de terceiros](#4-custom-code-e-scripts-de-terceiros)
5. [Interações e animações](#5-interações-e-animações)
6. [CSS / classes (o custo da bagunça)](#6-css--classes-o-custo-da-bagunça)
7. [Webflow: configurações de publicação](#7-webflow-configurações-de-publicação)
8. [Checklist antes de publicar](#8-checklist-antes-de-publicar)

---

## 1. As métricas que importam (Core Web Vitals)

O PageSpeed/Lighthouse mede principalmente:

| Métrica | O que é | Alvo | Principal causa de falha |
|---|---|---|---|
| **LCP** (Largest Contentful Paint) | Tempo até o maior elemento aparecer | < 2.5s | Imagem do hero pesada / fonte bloqueante |
| **CLS** (Cumulative Layout Shift) | Quanto o layout "pula" carregando | < 0.1 | Imagens sem dimensão, fontes trocando |
| **INP** (Interaction to Next Paint) | Resposta a cliques/toques | < 200ms | JS pesado, muitas interações Webflow |
| **TBT/FID** | Bloqueio da thread principal | baixo | Scripts de terceiros |

Foque o esforço em **LCP** e **CLS** — é onde sites Webflow mais perdem pontos.

---

## 2. Imagens — o maior ofensor

Imagens costumam ser 70%+ do peso de uma página Webflow. Regras:

1. **Formato:** sempre **WebP**. O Webflow converte para WebP automaticamente no publish — **garanta que a conversão está ativa** (Project Settings). Nunca suba PNG gigante de screenshot.
2. **Dimensione antes de subir.** Não suba 4000px para exibir em 600px. Redimensione na origem.
3. **Responsive images:** o Webflow gera `srcset` automaticamente para imagens colocadas como `Image` (não como background). **Prefira tag Image** a background-image quando o conteúdo for foto.
4. **Lazy load:** mantenha `loading="lazy"` (padrão do Webflow) em **tudo abaixo da dobra**.
5. **A imagem do LCP (hero) deve carregar `eager`**, não lazy. Lazy no hero **piora** o LCP. No Webflow, ajuste o setting de loading da imagem do hero para "eager" e considere `fetchpriority="high"` via custom attribute.
6. **Sempre defina width/height** (ou aspect-ratio) para evitar CLS — o navegador reserva o espaço antes da imagem chegar.
7. **SVG para ícones e logos.** Nunca PNG para ícone. SVG inline herda `currentColor`.

> Regra de bolso: nenhuma imagem individual acima de ~200KB sem justificativa. Hero idealmente < 150KB em WebP.

---

## 3. Fontes

Fontes bloqueiam render e afetam LCP/CLS.

1. **Só os pesos usados.** DM Sans tem 100–1000; carregamos **400 / 500 / 700**. Cada peso/itálico extra é download. Revise em Project Settings → Fonts.
2. **`font-display: swap`** para o texto aparecer com fallback antes da fonte carregar (evita texto invisível). O Google Fonts já manda `&display=swap`.
3. **Considere self-hosting** da DM Sans (subir os `.woff2` no Webflow) em vez de Google Fonts — elimina uma conexão a domínio externo e melhora LCP.
4. **Preload** do `.woff2` principal (peso 400) via custom code no `<head>` para o texto da dobra.
5. **Fallback metric-matched:** a stack `"DM Sans", Helvetica, Arial` já dá fallback próximo, reduzindo o "pulo" (CLS) quando a fonte troca.

---

## 4. Custom code e scripts de terceiros

Scripts de terceiros são o assassino silencioso de INP/TBT.

1. **Auditar tudo que está em Project Settings → Custom Code e por página.** Cada `<script>` externo (analytics, chat, pixels, A/B) custa.
2. **`defer` ou `async`** em todo script não-crítico. Nunca script bloqueante no `<head>` sem necessidade.
3. **Carregue chat/widgets sob demanda** (depois do load, ou no primeiro scroll/clique) em vez de no boot.
4. **Um Google Tag Manager** centralizando pixels é melhor que 5 scripts soltos — mas o GTM também pesa; só o necessário.
5. **Remova o que não usa.** Pixel de campanha antiga, biblioteca de slider que nem está mais na página — fora.

---

## 5. Interações e animações

As **Interactions** do Webflow viram JS. Em excesso, derrubam o INP.

1. **Prefira CSS** a interação Webflow quando possível: hover, transições simples → faça com `transition` na classe, não com interação JS.
2. **Anime só `transform` e `opacity`.** Animar `width`, `height`, `top`, `margin` força reflow/repaint a cada frame (travado). `transform: translate/scale` e `opacity` rodam na GPU.
3. **Cuidado com scroll-based interactions** em muitos elementos — cada uma adiciona listener de scroll. Use com moderação.
4. **Sem autoplay de vídeo pesado** na dobra. Se precisar, use poster + vídeo otimizado, ou lazy.

---

## 6. CSS / classes (o custo da bagunça)

Aqui o style guide e a performance se encontram. O CSS do site atual está inchado (centenas de classes órfãs e duplicadas geram um `.css` enorme — parte do problema de PageSpeed).

1. **Reutilize classes** (a doutrina BEM-Webflow do [00-nomenclatura](00-nomenclatura.md)). Menos classes únicas = CSS menor = download menor.
2. **Combo classes em vez de duplicar Blocks** — `.c-hero.is--dark` não duplica a base; `.c-hero-dark` duplicaria.
3. **Limpe classes não usadas** periodicamente: Webflow → "Clean up" no Style Manager remove classes sem uso. Faça antes de publicar marcos.
4. **Não empilhe utilities estilo Tailwind** (specificity hell + CSS maior). Componha com combo classes.

> Cada classe órfã vira regra CSS que todo visitante baixa. A padronização **é** otimização de performance.

---

## 7. Webflow: configurações de publicação

Em **Project Settings**, garanta:

- [ ] **Minify** HTML, CSS e JS ativados (Publishing / Advanced).
- [ ] **Responsive images / WebP** ativados.
- [ ] **SSL** ativo.
- [ ] **CDN** (nativo do Webflow) — sem ação, mas confirme que assets vêm do `cdn.prod.website-files.com`.
- [ ] Remover o badge/CSS de features Webflow não usadas, se aplicável.

---

## 8. Checklist antes de publicar

- [ ] Rodei o **PageSpeed Insights** (mobile **e** desktop) na URL de staging
- [ ] LCP < 2.5s — imagem do hero é WebP, eager, dimensionada
- [ ] CLS < 0.1 — todas as imagens têm dimensão; fonte com fallback
- [ ] Nenhuma imagem > 200KB sem justificativa
- [ ] Só pesos de fonte 400/500/700 carregando
- [ ] Scripts de terceiros auditados, com `defer`/`async`, sem órfãos
- [ ] Animações só com `transform`/`opacity`
- [ ] "Clean up" de classes não usadas rodado
- [ ] Minify ativado

> Meça **antes e depois** de cada marco. "Achismo de performance" é como o site chegou onde chegou.

---

**Anterior:** [← 09 — Responsividade](09-responsividade.md) | **Próximo:** [11 — Fluxo de Trabalho →](11-fluxo-de-trabalho.md)
