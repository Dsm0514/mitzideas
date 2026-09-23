---
name: MitzIdeas
description: Sistema editorial minimalista para um blog pessoal multilíngue.
colors:
  paper: "#F7F6F3"
  paper-muted: "#EFEDE8"
  surface: "#FAFAF8"
  ink: "#1C1B18"
  ink-secondary: "#5C5A54"
  ink-muted: "#9C9A93"
  moss: "#3D6B5E"
  moss-hover: "#2E5248"
  rule: "#E2DFD8"
  rule-hover: "#C8C4BB"
  tag-surface: "#E8E5DE"
  tag-ink: "#4A4840"
  night: "#18181A"
  night-muted: "#222226"
  night-surface: "#1E1E21"
  night-ink: "#E8E6E0"
  night-ink-secondary: "#A8A6A0"
  night-ink-muted: "#68665F"
  mint: "#6BAF9E"
  mint-hover: "#88C4B5"
  night-rule: "#2E2E32"
typography:
  display:
    fontFamily: "DM Sans, system-ui, sans-serif"
    fontSize: "2.5rem"
    fontWeight: 700
    lineHeight: 1.2
    letterSpacing: "-0.04em"
  headline:
    fontFamily: "DM Sans, system-ui, sans-serif"
    fontSize: "2rem"
    fontWeight: 600
    lineHeight: 1.25
    letterSpacing: "-0.02em"
  title:
    fontFamily: "DM Sans, system-ui, sans-serif"
    fontSize: "1.2rem"
    fontWeight: 700
    lineHeight: 1.7
    letterSpacing: "-0.02em"
  body:
    fontFamily: "Lora, Georgia, serif"
    fontSize: "16px"
    fontWeight: 400
    lineHeight: 1.7
  label:
    fontFamily: "DM Sans, system-ui, sans-serif"
    fontSize: "0.8rem"
    fontWeight: 600
spacing:
  compact: "0.5rem"
  default: "0.75rem"
  field: "1rem"
  section: "1.5rem"
  article: "2rem"
  page: "3rem"
rounded:
  small: "5px"
  standard: "8px"
  pill: "999px"
components:
  search-field:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.ink}"
    rounded: "{rounded.standard}"
    padding: "0.7rem 1rem 0.7rem 2.75rem"
  filter-select:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.ink-secondary}"
    rounded: "{rounded.small}"
    padding: "0.4rem 2rem 0.4rem 0.75rem"
  tag:
    backgroundColor: "{colors.tag-surface}"
    textColor: "{colors.tag-ink}"
    rounded: "{rounded.pill}"
    padding: "0.15rem 0.45rem"
  dropdown:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.ink-secondary}"
    rounded: "{rounded.standard}"
    padding: "0.375rem"
---

# Design System: MitzIdeas

## Overview

**Creative North Star: “O Caderno Calmo”.**

MitzIdeas é uma experiência editorial pessoal, feita para leitura pausada e para o acervo de ideias ganhar valor com o tempo. A interface quase desaparece: linhas sutis organizam, uma tipografia serifada conduz a leitura e o verde-musgo aparece como orientação discreta, não como elemento promocional.

A densidade é baixa fora do texto e confortável dentro dele. A home organiza posts em uma cronologia simples; o artigo oferece uma coluna de leitura ampla e um sumário lateral apenas quando há espaço. O sistema troca de tema sem trocar de personalidade: papel quente e tinta suave no claro, carvão e menta desaturada no escuro.

**Key Characteristics:**

- editorial, minimalista e pessoal;
- contraste suave, sem branco ou preto puros;
- interface em DM Sans e prosa em Lora;
- largura de leitura contida, divisores em vez de cards;
- estados comunicados por mudança sutil de superfície, cor ou borda.

## Colors

O claro é uma paleta de papel quente; o escuro é carvão discreto. O acento é reservado a links, foco e detalhes de orientação.

### Tema claro

- **Paper** (`#F7F6F3`): fundo principal da página.
- **Muted Paper** (`#EFEDE8`): superfície de hover, bloco de citação e fundo secundário.
- **Surface** (`#FAFAF8`): campos de busca, selects e menus flutuantes.
- **Ink** (`#1C1B18`): texto e títulos principais.
- **Secondary Ink** (`#5C5A54`): navegação e texto auxiliar.
- **Faded Ink** (`#9C9A93`): placeholders, datas, contadores e rodapé.
- **Moss** (`#3D6B5E`): links, acento e estado de foco.
- **Deep Moss** (`#2E5248`): hover de links.
- **Rule** (`#E2DFD8`): bordas e divisores.
- **Hover Rule** (`#C8C4BB`): metadados e contraste secundário.
- **Tag Surface / Tag Ink** (`#E8E5DE` / `#4A4840`): categorias e filtros ativos.

### Tema escuro

- **Night** (`#18181A`): fundo principal.
- **Night Muted** (`#222226`) e **Night Surface** (`#1E1E21`): hover e componentes elevados.
- **Night Ink** (`#E8E6E0`), **Night Secondary** (`#A8A6A0`) e **Night Faded** (`#68665F`): a mesma hierarquia tipográfica do claro.
- **Mint** (`#6BAF9E`) e **Light Mint** (`#88C4B5`): acento e hover de link.
- **Night Rule** (`#2E2E32`): divisor e borda; **Night Hover Rule** é `#44444A`.

**The Quiet Accent Rule.** O musgo/menta não deve virar fundo dominante, CTA grande ou cor decorativa recorrente. Ele aponta para ações e links; a página continua pertencendo ao texto.

## Typography

**Display & UI Font:** DM Sans, `system-ui`, sans-serif.  
**Reading Font:** Lora, Georgia, serif.

**Character:** DM Sans torna navegação, filtros, datas e títulos objetivos e compactos. Lora mantém o corpo humano, confortável e literário. Não inverta as famílias.

### Hierarchy

- **Home display** (DM Sans, 700, `2.5rem`, tracking `-0.04em`): título da hero da home; reduz para `1.75rem` em até `640px`.
- **Article title** (DM Sans, 600, `2rem`, line-height `1.25`): título de post, página ou lista.
- **Section heading** (DM Sans, 600, `1.4rem`): `h2` no conteúdo.
- **Subheading** (DM Sans, 600, `1.15rem`): `h3` no conteúdo.
- **Chronology title** (DM Sans, 700, `1.2rem`, tracking `-0.02em`): mês/ano da listagem.
- **Body** (Lora, 400, `16px`, line-height `1.7`): prosa, links em contexto e leitura longa.
- **UI label** (DM Sans, 600, `0.8rem`): datas, selects, contadores e navegação compacta.
- **Metadata / ToC** (DM Sans, `0.85rem`): data, categoria, série e sumário.
- **Tag** (DM Sans, 500, `0.7rem`): categoria de post; filtro ativo usa `0.75rem`.

**The Reading-First Rule.** Corpos de texto permanecem em Lora a `16px/1.7`; não comprima artigos com fonte de UI, tracking amplo ou largura sem limite.

## Layout

O cabeçalho tem `56px`, padding lateral de `1.5rem`, é sticky em `top: 0` e usa blur de `8px`. No mobile o padding cai para `0.75rem` e os controles encolhem; até `360px`, o link “Sobre” e o divisor saem para preservar os ícones.

A home tem hero central com `3.5rem 1.5rem 2rem`, seguida por busca e posts em uma coluna de largura máxima `760px`. A listagem usa grupos mensais e linhas, não cards. A página Sobre limita-se a `680px`.

Artigos usam um container de no máximo `1140px`, coluna principal de no máximo `720px`, gap de `5rem` e sumário de `240px`. O sumário só aparece a partir de `1024px`; abaixo disso a leitura continua com uma coluna. O ritmo comum é `0.5rem`, `0.75rem`, `1.5rem`, `2rem`, `2.5rem` e `3rem`.

## Elevation & Depth

O sistema é plano por padrão. A separação vem de superfícies tonais, bordas de `1px` e espaço em branco. Sombras são contidas e existem somente em campos, imagens e menus flutuantes.

### Shadow Vocabulary

- **Field / quiet image** (`0 1px 3px rgba(0,0,0,0.06)` no claro; alpha `0.2` no escuro): campo de busca em repouso.
- **Dropdown / image** (`0 4px 12px rgba(0,0,0,0.08)` no claro; alpha `0.3` no escuro): menu de tema/idioma e imagens do artigo.
- **Focus ring** (`0 0 0 3px rgba(61, 107, 94, 0.12)`): foco do campo de busca no tema claro.

**The Flat-By-Default Rule.** Não use sombras para transformar cada bloco em card. Um menu aberto, uma imagem e um input podem ter profundidade; a cronologia e a prosa não.

## Shapes

O sistema usa cantos discretos: `8px` para campos, menus, imagens, código em bloco e citações; `5px` para controles compactos e linhas com hover; `999px` exclusivamente para tags. Bordas são sempre suaves e usam `var(--border)`. Citações são a única forma assimétrica: borda esquerda de `3px` em acento e raio no lado direito.

## Components

### Header and navigation

- **Shell:** fundo igual ao da página, divisor inferior e posição sticky.
- **Brand:** DM Sans 700, `1.1rem`, tracking `-0.03em`; `1rem` no mobile.
- **Text link:** DM Sans 500, `0.875rem`, padding `0.4rem 0.75rem`, raio `5px`; hover troca texto para primário e aplica fundo secundário.
- **Icon control:** área de `34px` por `34px` (`30px` no mobile), sem borda nem fundo em repouso; hover usa fundo secundário.
- **Dropdown:** `8px` de raio, borda, sombra média, padding `0.375rem`; abre abaixo do botão com afastamento de `6px`.
- **Dropdown option:** DM Sans `0.875rem`, padding `0.45rem 0.75rem`; hover e seleção ativa são iguais: fundo secundário + texto primário.

### Search and filters

- **Search:** altura definida pelo padding `0.7rem`; ícone de 16px posicionado à esquerda e texto em DM Sans `0.9rem`.
- **Focus:** remove o outline do navegador, aplica borda de acento e focus ring. Não há variação de erro, sucesso ou disabled no CSS atual.
- **Clear action:** não existe em repouso; aparece somente depois que há texto e escurece no hover.
- **Selects:** compactos, de `0.8rem`, com seta SVG própria; foco troca apenas a borda para o acento. Em até `640px`, empilham verticalmente.
- **Active filter:** pill de DM Sans, fundo de tag, `0.75rem`, com ação de remoção interna sem fundo.

### Post chronology

- **Month heading:** DM Sans 700, `1.2rem`, divisor inferior de `2px` e padding inferior de `0.5rem`.
- **Post row:** flex alinhado à baseline, gap `0.75rem`, borda inferior de `1px` e padding vertical de `0.5rem`.
- **Row hover:** fundo secundário, expansão lateral de `0.75rem`, raio `5px`; a borda inferior fica transparente.
- **Date:** DM Sans 600, `0.8rem`, texto esmaecido e largura mínima de `24px`.
- **Title link:** Lora `0.95rem`, cor primária; no hover muda apenas para o acento.
- **Category pill:** DM Sans 500, `0.7rem`, fundo de tag; some no mobile.

### Article content

- **Article header:** `3rem` no topo, `2rem` abaixo e borda inferior; título a `2rem/1.25`.
- **Metadata:** DM Sans `0.85rem`, texto esmaecido, `0.75rem` entre itens e ponto divisor.
- **Blockquote:** fundo secundário, borda esquerda de acento, padding `0.75rem 1.25rem` e raio assimétrico.
- **Inline code:** fundo secundário, borda, raio `5px`, padding `0.15em 0.4em`, acento para o texto.
- **Code block:** fundo secundário, borda, raio `8px`, padding `1.25rem`, scroll horizontal quando necessário. No tema claro, cores de syntax highlight são removidas para preservar a tinta principal.
- **Image:** largura máxima de `100%`, raio `8px`, sombra média.
- **Table of contents:** coluna silenciosa em DM Sans; título em caixa alta `0.8rem`, tracking `0.05em`; links em `0.85rem` e somente a cor muda no hover.

### Footer

O rodapé é um divisor superior + padding `1.5rem`, alinhamento central e DM Sans `0.8rem` em texto esmaecido. Ele não deve ganhar badges, links promocionais ou uma superfície independente.

## Do's and Don'ts

### Do:

- **Do** reutilize variáveis `--bg-*`, `--text-*`, `--accent`, `--border`, `--radius` e sombras de `assets/css/custom.css`.
- **Do** mantenha tema claro e escuro por variáveis, adicionando valores correspondentes em `:root` e `.dark` quando um novo token for realmente necessário.
- **Do** prefira linhas cronológicas, divisores e espaços a coleções de cards.
- **Do** preserve a coluna de leitura de até `720px` e a diferença entre tipografia de interface e de prosa.
- **Do** forneça estado de hover/focus para controles interativos seguindo a transição existente de `0.1s` ou `0.15s`.

### Don't:

- **Don't** use branco/preto puros, azuis genéricos de interface, gradientes ou cores saturadas que não existam no sistema.
- **Don't** criar variantes de botão primário, perigo, loading, erro ou disabled como se já fossem parte do produto; elas não existem no CSS atual.
- **Don't** introduzir bordas grossas, raios grandes, shadow cards ou esquemas de dashboard.
- **Don't** substituir Lora por sans-serif no texto editorial, nem usar DM Sans com peso alto em cada linha de conteúdo.
- **Don't** expor o sumário lateral em telas menores que `1024px` ou categorias em linhas de post menores que `640px`.
