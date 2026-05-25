# Style Guide Webflow — turn.io

> Documentação de padronização para o rebuild do site turn.io no Webflow. Objetivo: que qualquer pessoa da equipe saiba como **nomear, montar e otimizar** — sem depender de explicação pessoal.

---

## Por onde começar

Caminho essencial para começar a construir (4 docs):

**[00 Nomenclatura](docs/00-nomenclatura.md)** → **[01 Tokens](docs/01-tokens.md)** → **[03 Layout](docs/03-espacamento-layout.md)** → **[04 Componentes](docs/04-componentes.md)**

Os arquivos estão **numerados na ordem de leitura**: dá para seguir de `00` a `11` em sequência (cada doc tem um rodapé *Anterior / Próximo*) ou pular direto pelo índice abaixo.

---

## Índice da documentação

A doc é organizada em três camadas: **Fundamentos** (00–03) → **Construção** (04–08) → **Qualidade & Processo** (09–11).

| # | Documento | O que cobre |
|---|---|---|
| 00 | [Nomenclatura BEM](docs/00-nomenclatura.md) | Convenção Block / Element / Modifier, combo classes, namespaces |
| 01 | [Design Tokens](docs/01-tokens.md) | Cores (paleta real do turn.io), spacing, radius, sombras como Webflow Variables |
| 02 | [Tipografia](docs/02-tipografia.md) | DM Sans, escala tipográfica, text styles reutilizáveis, fluid type |
| 03 | [Espaçamento & Layout](docs/03-espacamento-layout.md) | `l-page`, `c-section`, `l-container`, grids, stacks |
| 04 | [Componentes](docs/04-componentes.md) | Biblioteca completa (20+ componentes) com o CSS de cada classe |
| 05 | [Formulários](docs/05-formularios.md) | Inputs, select, checkbox, radio, validação, estados Webflow |
| 06 | [Utilitários](docs/06-utilitarios.md) | Classes `u-*`: visibilidade, flex, espaçamento, a11y, truncate |
| 07 | [CMS & Rich Text](docs/07-cms-richtext.md) | Blog dinâmico: estilizar Rich Text, Collection List, card de post |
| 08 | [Combo Classes](docs/08-combo-classes.md) | Catálogo de modifiers `is--`, combo class vs Webflow Variants |
| 09 | [Responsividade](docs/09-responsividade.md) | Breakpoints Webflow, cascata, estratégia desktop-first |
| 10 | [Performance](docs/10-performance.md) | Core Web Vitals, imagens, fontes, scripts — **o motivo do rebuild** |
| 11 | [Fluxo de Trabalho](docs/11-fluxo-de-trabalho.md) | Governança: criar componente/página, naming review, decisões em aberto |

---

## Princípios do projeto

O "porquê" detalhado está em cada doc; em uma frase cada:

- **Zero valor cru.** Cor, spacing, fonte e radius sempre via Variable/classe — nunca hex ou px direto no elemento ([01](docs/01-tokens.md)).
- **Nomes semânticos BEM.** Toda classe diz o que é: `c-` componente, `__` elemento, `is--` variação ([00](docs/00-nomenclatura.md)).
- **Reutilizar > criar.** Variação é combo class `is--`, não Block duplicado. Menos CSS único = site mais rápido ([08](docs/08-combo-classes.md)).
- **Performance é decisão de construção**, não otimização de fim de projeto — foi a falta disso que motivou o rebuild ([10](docs/10-performance.md)).

---

## Estrutura do repositório

```
.
├── README.md            ← você está aqui (índice + por onde começar)
└── docs/
    ├── 00-nomenclatura.md
    ├── 01-tokens.md
    ├── 02-tipografia.md
    ├── 03-espacamento-layout.md
    ├── 04-componentes.md
    ├── 05-formularios.md
    ├── 06-utilitarios.md
    ├── 07-cms-richtext.md
    ├── 08-combo-classes.md
    ├── 09-responsividade.md
    ├── 10-performance.md
    └── 11-fluxo-de-trabalho.md
```

---

## Como manter esta documentação

- **Numeração = ordem de leitura.** Ao inserir um doc novo no meio, renumere a sequência e ajuste os rodapés *Anterior / Próximo* dos vizinhos.
- **Mantenha a corrente de navegação.** Cada doc liga ao anterior e ao próximo; o `00` e o `11` fecham o ciclo apontando de volta para este README.
- **Convenção mudou?** Atualize o doc correspondente — e o [00 Nomenclatura](docs/00-nomenclatura.md) se for regra de nome de classe.

---

*Mantido pela equipe de desenvolvimento. Atualize quando as convenções do projeto evoluírem.*
